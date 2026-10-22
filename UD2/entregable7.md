## ✅ Parte A · Hecha en Entregable 5

| MALWARE | RESUMEN |
|---------|----------|
| Adware | Muestra anuncios no deseados, Secuestra el navegador y Rastrea tu actividad |
| Backdoor | Mecanismo oculto que permite saltarse los sistemas habituales de autenticación y seguridad para acceder a un SI, red o aplicación |
| Bomba Fork | Denegación de servicios (DoS), Autorreplicación infinita , Agotamiento de recursos, Bloqueo total |
| Bots | Programa diseñado para automatizar tareas repetitivas en Internet de forma mucho más rápida que un humano |
| Bug | Error, Defecto o Incorreción en el código fuente de un programa que hace que se comporte de manera imprevista, arroje resultados incorrectos o deje de funcionar |
| Caballo de Troya | Se disfraza de programa legítimo e inofensivo para engañar al usuario y lograr que lo instale de forma voluntaria |
| Cookies | Pequeño archivo de texto que un sitio web almacena en tu navegador cuando lo visitas, para que pueda recordar sobre ti en futuras visitas |
| Crackers | Experto en informática que usa sus conocimientos para violar la seguridad de sistemas, romper medidas antipiratería o crear SW malicioso |
| Cryptovirus, Ransomware | Bloquea y cifra los archivos de un dispositivo para exigir un rescate a cambio de devolver el acceso |
| Exploits | Fragmento de código, programa o secuencia de comandos diseñado para aprovechar una vulnerabilidad o fallo de seguridad en un SW, SOp o HW con el fin de lograr un comportamiento no previsto |
| Hijacker | Secuestrador de Navegador, Malware que toma el control de tu navegador web sin tu consentimiento, alterando su configuración para dirigir tu navegación hacia sitios web no deseados |
| Hoax, Joke o Bulo | Falsa alarma o engaño masivo difundido por Internet o RRSS que no contiene código malicioso directo (Ingeniería Social) |
| Keystroke o Keylogger | SW o HW diseñado para interceptar y registrar cada tecla que presionas en tu teclado |
| Ladilla virtual | Programa molesto o Adware/Joke que causa interrupciones constante en el equipo
| Leapfrog | Estrategia o táctica donde se salta una etapa intermedia para avanzar de forma directa a un estado superior |
| Parásito Informático | Malware que necesita acoplarse e infectar archivos ejecutables o sectores de arranque ya existentes en el equipo anfitrión para funcionar y propagarse |
| Pharming | Manipulación del tráfico de Internet para redirigirte a un sitio web falso | 
| Phising | Correos maliciosos | 
| Rabbit o Conejos | Malware diseñado para reproducirse sin control de forma local, multipolicando sus propios archivos o procesos en un PC hasta colapsar el sistema |
| Riskware | Programas legítimos que no han sido creados para ser maliciosos, pero que contienen funciones que un atacante puede aprovechar para violar la seguridad de tu sistema |
| Rootkit | Conjunto de programas de herramientas de SW diseñadas para ocultar la presencia de programas maliciosos y mantener un acceso privilegiado y oculto en un SOp |
| Scumware o Escoria | Modifica o altera las páginas web que visitas en tiempo real |
| Spam | Envío masico, no solicitado y repetitivo de mensajes |
| Spyware | Recopila información sobre ti o tu dispositivo de manera secreta y la envia a terceros sin tu conocimiento ni consentimiento |
| Pop_Ups o Ventanas emergentes | Ventana que se abre de forma automática e imprevista en la pantalla de tu navegador o SOp |
| Worms o Gusanos | Se duplica a sí mismo y se propaga automáticamente e un PC a otro a través de la red |

## ✅ Parte B · Preparar un entorno controlado (CE d)

Rellenad en vuestro documento:

| Medida de aislamiento |	¿Hecho?	| ¿Qué riesgo evita? | 
|-----------------------|---------|--------------------|
| Sin carpetas compartidas ni portapapeles | Sí | Evita que el malware escape al equipo anfitrión |
| Snapshot previo | Sí | Evita daños permanentes permitiendo restaurar el estado limpio |
| Sin red (o red interna aislada) al ejecutar | Sí | Evita la comunicación con servidores externos y el robo de datos |
| Máquina que no contiene datos personales ni credenciales | Sí | Evita el robo de información confidencial o contraseñas reales |

## ✅ Parte C · El fichero EICAR: una amenaza de mentira (CE e)

### Anotad: ¿cuántos motores lo detectan?

Lo detectan 66 de 68 motores antivirus en VirusTotal como la amenaza de prueba estándar EICAR-Trst-File.

<img width="1908" height="302" alt="image" src="https://github.com/user-attachments/assets/ecab379b-6d55-4e7b-8417-eff7265be7d6" />

## ✅ Parte D · Instalar y usar ClamAV (CE e)

### 🚀 D1. Instalación (con NAT activo)

### 🚀 D2. Actualizar las firmas (la "base de datos de virus"). Si falla con un mensaje de bloqueo, parad antes el servicio automático:

Anotad la fecha de la base de datos: 
```
📌 $ clamscan --version
```
* **Fecha:** 22 de octubre de 2026

### 🚀 D3. Escanear el fichero EICAR:
```
📌 $ clamscan ~/eicar.txt
```

#### Veréis una línea terminada en FOUND y un resumen. Anotad el nombre con el que ClamAV bautiza la amenaza y el número de ficheros infectados.

<img width="568" height="316" alt="image" src="https://github.com/user-attachments/assets/440cc146-12bf-403f-a6af-a85d1183e46c" />

EL nombre con el que ClamAV bautiza la amenzaza es -> Eicar-Test-Signature y el nº de ficheros infetcados es 1.

### 🚀 D4. Escanear una carpeta entera, mostrando solo lo infectado:
```
📌 $ clamscan -r -i -v ~
```

| Opción	| Qué hace |
|---------|----------|
| -r	| Recursivo |
| -i	| Muestra solo ficheros infectados |
| -v	| Modo verboso |
| --move=DIR	| Mueve los infectados a una carpeta (cuarentena) | 
| --remove	| Borra los infectados (peligroso con falsos positivos) |
| -l fichero.log | Guarda un registro | 



### 🚀 D5. Cuarentena en vez de borrar
```
📌 $ mkdir -p ~/cuarentena
📌 $ clamscan -r -i -v --move=$HOME/cuarentena ~
📌 $ ls -l ~/cuarentena
```

#### Pregunta D: ¿Por qué suele ser mejor mover a cuarentena que borrar directamente? ¿Qué pasaría si ClamAV marcara por error un fichero importante del ERP de Torrent?

