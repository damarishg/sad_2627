# 1. Tabla de ubicación

| Servicio | Zona | Justificación (una frase)|
|----------|------|--------------------------|
| Web corporativa pública | DMZ | Tiene que estar disponible fuera de la LAN |
| Servidor de correo | LAN | No pueden acceder a él desde Internet |
| Pasarela VPN | DMZ | Está expuesta a Internet pero está aislada de la LAN | 
|Proxy (saliente e inverso) | DMZ | Es el intermediario entre Internet y DMZ y entre DMZ y LAN |
| HMI de planta y carpeta de planos | Zona Crítica | Son archivos/documentos restringidos |
| Autoridad de certificación (srv-ca) | Zona Crítica | Solo tienen acceso a él personas autorizadas |
| Servidor de copias de seguridad | Zona crítica | Solo tienen acceso a él personas autorizadas |
| Portátiles de ingenieros y operarios | LAN | Son equipos confiables |
| Wi-Fi de invitados | DMZ | Está expuesto a Internet pero está aislado de la LAN |
| Servidor de registros y monitorización | Zona Crítica | Solo tienen acceso a él personas autorizadas |

# 2. Tabla de flujos

| Origen → Destino | ¿Permitido? | Ejemplo concreto |
|------------------|-------------|------------------|
|Internet → DMZ | Solo servicios publicados | Tener el puerto 443 de escucha para poder acceder a páginas con HTTPS |
|Internet → LAN | No | No debe haber conexión | 
| DMZ → LAN | Solo lo imprescindible y concreto | Una VPN |
| LAN → DMZ | Sí, para usar y administrar | Administrar la web por SSH |
| LAN → Internet | Sí, mejor a través de un proxy | Navegación de los usuarios | 

# 3. Diagrama de red

<img width="591" height="142" alt="image" src="https://github.com/user-attachments/assets/4fc74a19-2a8b-4325-8f89-b33bc685c5da" />

# 4. Pregunta de reflexión
## Si cae el servidor web, ¿qué puede hacer el atacante en tu diseño y qué se lo impide?
Si el servidor web está caído el atacante no puede acceder a un servicio que no está operativo, además, los cortafuegos siguen activos. E 1º bloquea el tráfico no autorizado desde Internet hacia la DMZ y el 2º impide avanzar desde la DMZ hacia la LAN interna.
