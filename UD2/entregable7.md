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

<img width="460" height="60" alt="image" src="https://github.com/user-attachments/assets/4c4309ec-5642-4e1c-8ad1-a6200a600333" />


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

<img width="407" height="273" alt="image" src="https://github.com/user-attachments/assets/03700f26-f974-4db6-b0c6-377f04e320e7" />


### 🚀 D5. Cuarentena en vez de borrar
```
📌 $ mkdir -p ~/cuarentena
📌 $ clamscan -r -i -v --move=$HOME/cuarentena ~
📌 $ ls -l ~/cuarentena
```

<img width="562" height="100" alt="image" src="https://github.com/user-attachments/assets/899a1313-9d3b-4f6a-a7d2-39020cd945e6" />


#### Pregunta D: ¿Por qué suele ser mejor mover a cuarentena que borrar directamente? ¿Qué pasaría si ClamAV marcara por error un fichero importante del ERP de Torrent?

* **¿Por qué suele ser mejor mover a cuarentena que borrar directamente?**

Evita pérdidas por falsos positivos, si el antivirus se equivoca con un archivo legítimo, al estar en cuarentena no se destruye de forma permanente. Permite investigar, congela la amenaza para que el analista de seguridad pueda estudiarla. Y permite devolver el archivo a su sito inmediatamente si se confirma que es seguro.

* **¿Qué pasaría si ClamAV marcara por error un fichero importante del ERP de Torrent?**

El programa ERP dejaría de funcionar, bloqueando la facturación, ventas o gestión de la empresa. Exigiría recurrir a copias de seguridad o reinstalar el sistema, provocando horas de parada en lugar de una simple restauración desde cuarentena.

#### ¿Para qué sirve la cuarentena?

Sirve para aislar de forma segura el archivo infectado impidiendo que dañe el sistema. A diferencia del borrado directo, permite prevenir la pérdida de datos si se trata de un falso positivo (pudiendo restaurarlo al instante) y conservar el fichero en un entorno controlado para su posterior análisis forense

### D6. Detectar rootkits (herramienta complementaria, también de la familia "paliativa"):
```
📌 $ sudo apt install -y rkhunter
📌 $ sudo rkhunter --update
📌 $ sudo rkhunter --propupd
📌 $ sudo rkhunter --check --sk --rwo
```
#### Avisos y vuestra valoración

<img width="883" height="122" alt="image" src="https://github.com/user-attachments/assets/ee7aae50-5e8d-4647-89d6-8991df56b1fb" />

**Valoración:** Todos los avisos son falsos positivos, el sistema está limpio y seguro.

**⚠️Aviso 1 (lwp-requeest):** Este programa de gestión de peticiones web instala por defecto un script de Perl y no como binario ejecutable tradicional. Rkhunter lo marca como sospechoso por precaución pero es un archivo legítimo.

**⚠️ Avisos 2, 3 y 4 (postfix y postdrop):** Ocurren porque acavo de instalar el paeuete rkhunter que trajo consigo la herramienta de correo postfix como paquete de dependencia. Al instalarse, el sistema ha creado automáticamente el usuario de postfix y los grupos postfix y postdrop. Son cambios legítimos realizados por el gestor de paquetes apt.

**⚠️ Avisos 5 y 6 (Archivos ocultos en /etc):** /etc/.updat y /etc/.resolv.conf.systemd-resolved.bak son archivos temporales de respaldo creados por le propio SOp para gestionar la red y el estado del sistema. No contienen nada malicioso.


## ✅ Parte E · Análisis estático: mirar sin ejecutar (CE d)

#### Contestad por escrito, antes de ejecutarlo:
* **1-Qué crees que hace cada línea?**
  
📁 File-> examina el tipo de puerto sin ejecutarlo (determina si es un script Shell de texto, un binario ejecutable, etc)

📁 sha256sum -> calcula el hash del archivo para comprobar su integridad o contrastarla con BBDD de malware

📁 cat -> muestra por pantalla todo el código del script en texto plano para poder leer sus instrucciones antes de ejecutarlo.

* **2- ¿Crea ficheros? ¿Abre puertos? ¿Modifica algo para sobrevivir a un reinicio?**
  
Creación de ficheros: Sí, crea la carpeta oculta /tmp/.cache_sistema, un registro (estado.log) y una copia de sí mismo (actualizador.sh).

Puertos: Sí, levanta un servidor web local en el puerto 8081 con Python (127.0.0.1)

Persistencia: Sí, añade una regla @reboot en el crontab para ejecutarse automáticamente tras cada reinicio.

🛑 ** NO SABÍA A QUE TE REFERÍAS EN ESTE APRTADO, SI AL .SH O A LO QUE HACEN LOS COMANDOS ANTERIORES**

* **3- Si fuera un .exe binario, ¿qué habría que hacer en su lugar? (Pista: no se puede leer, habría que usar herramientas como strings o un desensamblador.)**

Usaría strings para extraer las cadenas de texto legibles incrustadas y emplear herramientas de ingeniería inversa para analizar el código máquina sin ejecutarlo.


## ✅ Parte F · Análisis dinámico: observar qué hace (CE d)

**PRUEBAS:**

<img width="747" height="150" alt="image" src="https://github.com/user-attachments/assets/c2486e63-7e50-4510-b544-575124f8e3af" />

<img width="722" height="165" alt="image" src="https://github.com/user-attachments/assets/bc161abc-b7c7-4c13-b404-87d379fd87a6" />


**🎯 6. Rellenad el informe de comportamiento:**

| Qué hace	| Evidencia (comando y salida) | ¿Qué fase del ataque recuerda? |
|-----------|------------------------------|--------------------------------|
| Crea ficheros o carpetas | Comando: diff ~/antes_tmp.txt ~/despues_tmp.txt Salida: Muestra la creación del directorio oculto (/tmp/.cache_sistema) y el ejecutable (/tmp/.cache_sistema/actualizador.sh )| Creación de backdoors |
| Abre algún puerto o proceso	| Comando: diff ~/antes_procesos.txt ~/despues_procesos.txt / Salida: Muestra nuevos PIDs en ejecución correspondientes a la carga útil | Apertura del backdoor |
| Intenta persistir (sobrevivir a un reinicio) | Comando: diff ~/antes_cron.txt ~/despues_cron.txt Salida: > @reboot /bin/bash /tmp/.cache_sistema/actualizador.sh | Persistencia |

**🎯 7. Comprobad la persistencia: reiniciad la VM y volved a mirar si el comportamiento reaparece.**

<img width="617" height="65" alt="image" src="https://github.com/user-attachments/assets/0d5cc8c3-4b5d-422e-9d04-ec17a73b856b" />

<img width="823" height="67" alt="image" src="https://github.com/user-attachments/assets/5599eb36-69d8-41b9-89af-e6bf01e6a641" />


Al reiniciar he comprobado con $ crontab -l que la tarea que programó el script sigue guardada, Además he hecho un $ ps aux para comprobar que el archivo actualizador.sh se ejecuta automáticamente.

**🎯 8. Erradicación: documentad cómo lo eliminaríais (parar el proceso, quitar la entrada de arranque, borrar los ficheros) y verificad con los mismos comandos del paso 3 que ha desaparecido. Después podéis volver al snapshot.**

**1- Parar el proceso activo:** 
```
📌 $ pkill -f actualizador.sh
```
**2- Quitar la entrada de arranque (Cron):**
```
📌 $ crontab -r
```
**3- Borrar los archivos maliciosos:**
```
📌 $ rm -rf /tmp/.cache.sistema
```

**Verificación**

El sistema ha vuelto a quedar limpio, el malware ha sido eliminado con éxito.

<img width="671" height="63" alt="image" src="https://github.com/user-attachments/assets/ef36927c-8c93-469f-a418-507bd43a6e9e" />

**Pregunta F: Habéis encontrado el comportamiento comparando "antes" y "después". ¿Cuál es el equivalente en un servidor real de Torrent si nadie hizo la foto inicial? ¿Qué relación tiene con el inventario de servicios que hicisteis en la práctica anterior?**

**🎯1. Si en un servidor real nadie guardó la "foto inicial", ¿cómo sabemos si ha cambiado algo?**

En un entorno real no guardan los archivos de antes y después. Normalmente Saben qué programas y puertos son los normales en el PC, además de tener programas de seguridad y registros que guardan todo lo que pasa y avisan si se crea un archivo o se abre un puerto sin que hayas hecho nada.

**🎯 2. ¿Qué relación tiene esto con el inventario de servicios de la práctica anterior?**

El inventario es el documento donde se apunta todo lo que debería estar funcionando en el servidor (servicios y puertos autorizados). Si revisas el servidor y ves un puerto o un proceso abierto que no está apuntado en el inventario sabes que es algo sospechoso y que no debería estar ahí.

## ✅ Parte G · Preventivo o paliativo (CE c, e)

Clasificad cada medida:

| Medida	| Preventiva |	Paliativa |
|---------|------------|------------|
| Mantener el sistema actualizado| ✅ | |
| ClamAV escaneando a demanda un disco | ✅ | |
| Descargar software solo de repositorios oficiales | ✅ | | 
| Copia de seguridad semanal desconectada de la red |  | ✅ |
| Cuenta de usuario sin privilegios de administrador | ✅ |  |
| rkhunter tras sospechar una intrusión |   | ✅ |
| Formación del personal contra el phishing | ✅ |  |
