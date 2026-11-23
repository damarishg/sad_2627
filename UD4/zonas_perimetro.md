# 1. Tabla de ubicación

| Servicio | Zona | Justificación (una frase)|
|----------|------|--------------------------|
| Web corporativa pública | DMZ | Tiene que estar disponible fuera de la LAN |
| Servidor de correo | LAN | No pueden acceder a él desde Internet |
| Pasarela VPN | DMZ | Porque 
|Proxy (saliente e inverso) | DMZ |
| HMI de planta y carpeta de planos | Zona Crítica |
| Autoridad de certificación (srv-ca) | Zona Crítica | 
| Servidor de copias de seguridad | Zona crítica |
| Portátiles de ingenieros y operarios | LAN |
| Wi-Fi de invitados | Internet | 
| Servidor de registros y monitorización | 
