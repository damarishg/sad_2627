## ✅ Parte A · ¿Está actualizado el sistema?
### 💡 A1. Anotad qué versión de sistema operativo y kernel tenéis:

```
📌 $ lsb_release -a
📌 $ uname -r
```
Versión del SOP -> Ubuntu 24.04.3 LTS

<img width="406" height="132" alt="image" src="https://github.com/user-attachments/assets/def9a0ac-0185-4ce9-ad4c-d6db92cf5d95" />

Versión del Kernel -> 7.0.0-31-generic

<img width="295" height="57" alt="image" src="https://github.com/user-attachments/assets/da55f410-8e5e-42d2-8f90-d266147f600d" />

### 💡 A2. Pedid al sistema la lista de paquetes disponibles y qué se puede actualizar:

```
📌 $ sudo apt update
📌 $ apt list --upgradable
```
<img width="800" height="92" alt="image" src="https://github.com/user-attachments/assets/5cdd99cc-e12b-4a29-b286-c28261664dbf" />


**Anotad: ¿cuántos paquetes tienen actualización pendiente?**

Se pueden actualizar 226 paquetes.


### 💡 A3. Simulad la actualización sin aplicarla:

```
📌 $ sudo apt-get -s upgrade |head -30
```
La opción -s es "simular". Fijaos en que dice qué instalaría, pero no toca nada.

<img width="667" height="126" alt="image" src="https://github.com/user-attachments/assets/e91746bd-c7bb-425b-852c-6954efb0182d" />

<img width="511" height="100" alt="image" src="https://github.com/user-attachments/assets/9d1411ac-0ddd-4cdb-9a57-17ad20e85fe6" />


### 💡 A4. ¿Y si el sistema ya no recibe parches? Buscad en https://endoflife.date la fecha de fin de soporte de:
* La versión de Ubuntu/Debian de vuestro Torrent-Auditor.
* Ubuntu 8.04, la que tiene Torrent-Vulnerable (Metasploitable2).

Rellenad:

| Sistema | Fecha de fin de soporte | ¿Recibe parches hoy? |
|---------|-------------------------|----------------------|
| Torrent-Auditor (Ubuntu 24.04.3) | 25 de abril de 2034 | Sí |
| Torrent-Vulnerable (Ubuntu 8.04) | 09 de mayo de 2013 | No |

### Pregunta A: En la oferta de empleo de Torrent (dossier de la UD02) decían que trabajaban con "Ubuntu 8.04". Aunque pusieran todas las contraseñas bien, ¿qué problema sigue sin tener solución?

El SOp no tiene soporte ni actualizaciones, tener un SOp con estas características es un error de seguridad muy grave. Como he dicho no tiene soporte y al no tener actualizaciones no tiene parches. Pueden usar Exploits para acceder al sistema sin ningún esfuerzo ni mucho conocimiento.

## ✅ Parte B · ¿De dónde vienen los programas instalados?
### 💡 B1. Mirad de qué repositorios se descarga vuestro sistema:

```
📌 $ cat /etc/apt/sources.list

📌 $ ls /etc/apt/sources.list.d/
```
 
Investiga el fichero donde están configurados los repositorios y comenta el significado de cada una de las líneas y su contenido.

<img width="655" height="270" alt="image" src="https://github.com/user-attachments/assets/d73a6fb5-1dd0-4b03-9830-9c7a3a97b759" />

<img width="481" height="57" alt="image" src="https://github.com/user-attachments/assets/0e6e1a34-82cb-4c8a-a71d-91cac664ed20" />

**1- Types:** Indica el tipo de repositorio. DEB significa que descargará paquetes precompilados e instalables.

**2- URIs :** Es la dirección web  (servidor espejo) desde donde se descargan los programas.

**3- Suites:** Es la versión específica de Ubuntu y las ramas de paquetes que va a buscar. 
* **NOBLE:** Es el nombre en clave de la versión de la versión de Ubuntu.

* **NOBLE-UPDTES:** Actualizaciones y correcciones que se lanzan tras la salida del sistema. 

* **NOBLE-BACKPORTS:** Versiones más recientes de ciertos programas adaptadas para esta versión.

* **NOBLE-SECURITY:** Parches de seguridad de seguridad específicos.

**4- Components:** Categorías o secciones de SW que tienes activas según sus licencias y soporte. 

🏁 MAIN: SW libre y de Código Abierto respaldado con soporte oficial de Canonical.

🏁 RESTRICTED: Controladores propietarios del sistema.

🏁 UNIVERSE: Programas libres mantenidos por la comunidad de Linux.

🏁 MULTIVERSE: Programas no libres y restringidos por patentes o derechos de autor.

**5- Signed-By:** Clave digital de Seguridad (clave pública de Ubuntu).

### 💡 B2. apt solo instala paquetes firmados por claves en las que confía. Mirad dónde están esas claves

<img width="812" height="292" alt="image" src="https://github.com/user-attachments/assets/f8f976fb-c3b1-4814-b2fa-1ad662667480" />

**Reflexiona: ¿qué pasaría si un atacante introduce una clave falsa en /usr/share/keyrings/? 
¿y si intercepta o reemplaza la clave oficial de Ubuntu ubuntu-archive-keyrings.gpg?**

* **/usr/share/keyrings/:** En el caso de que introduzca un a clave y tú descargues un paquete que tenga como clave la que ha puesto el atacante lo más probable es que estén instalando un SW con un maleware o algo que el atacante quiere que tu PC tenga.

* **ubuntu-archive-keyrings.gpg:** En el caso de que intercepte la clave oficial sería peor, ya que no hace falta descargar un SW específico, si no que con cualquier SW tú PC está en peligro.

### 💡 B3. Información de un paquete concreto. Elegid openssh-server o curl:

<img width="816" height="277" alt="image" src="https://github.com/user-attachments/assets/c96321cf-8550-4299-841f-f3122b9832bb" />

Anotad: versión instalada, versión candidata, repositorio de origen y el significado de la tabla de versiones.

**Versión instalda:** 1:9.6p1-3ubuntu13.19

**Versión candidata:** 1:9.6p1-3ubuntu13.19

**Repositorio de origen:** 

<img width="556" height="52" alt="image" src="https://github.com/user-attachments/assets/c1df3565-58c0-4d92-bc23-6225777f8a08" />


**Significado de la tabla:**  Los * indica la versión instalada actualmente en el sistema. Los números indican una prioridad, a mayor número mayor prioriad (500, 100). Primero mira en el nº 500 en el caso de que falle mira en el nº 100. 

**🧠 /var/lib/dpkg/status:** Es la BBDD local del sistema que confirma que el paquete ya se encuentra instalado físicamente.


### 💡 B4. Buscad paquetes instalados que no proceden de ningún repositorio (instalados a mano):

<img width="896" height="187" alt="image" src="https://github.com/user-attachments/assets/b28f8d7a-2c53-42ce-ac08-d2019a3ca847" />

Cada línea que salga merece una pregunta: ¿quién lo instaló, de dónde, y quién lo verificó?

**📌 Nessus**

Es el único que está marcado como [installed. local]

**❓ ¿Quién lo instaló?:** El administrador del sistema de forma manual.

**❓ ¿De dónde?:** Desde un archivo instalador local (.deb) descargado externamente (desde Internet) y no desde los repositorios oficiales de Ubuntu.

**❓ ¿Quién lo verificó?:** El propio administrador al descargarlo del fabricante. Al no ser un paquete de repositorio, el SOp no pudo  verificarlo de forma automática mediante las claves de confianza de Ubuntu.

**📌 Resto de paquetes**

**❓ ¿Quién lo instaló?:** El gestor de paquetes del sistema (apt) de forma automática al resolver dependencias requeridas por otras aplicaciones.

**❓ ¿De dónde?:** Proceden directamente de los repositorios oficiales de Ubuntu ( como noble, noble-updates o noble-security).

**❓ ¿Quién lo verificó?:** El SOp Ubuntu mediante verificación automática de firmas digitales y claves de autenticidad del repositorio oficial.

### 💡 B5. Comprobad si algún fichero de un paquete ha cambiado desde que se instaló:
```
📌 $ sudo dpkg -V
```
Si no sale nada, todo coincide con lo instalado.

Si salen líneas, la primera columna indica qué cambió (5 = el hash MD5 es distinto). Los ficheros de configuración (c) cambian de forma normal. Un binario (/usr/bin/...) modificado es una alarma

<img width="725" height="208" alt="image" src="https://github.com/user-attachments/assets/a7adb524-ce9d-487d-89bf-52f444a3aa0d" />

### Pregunta B: dpkg -V ¿detectaría un rootkit que además hubiera modificado la base de datos de dpkg? ¿Por qué conviene comparar con una copia guardada fuera del sistema? (Pista: recordad la fase 4 de la intrusión.)

La cuarta fase es la de borrar huellas, en este caso al modificar también la BBD de dpkg no ve ninguna modificación, por ende el rootkit no sería detectado. Conviene tener una copia fuera porque en el caso de duda de modificación la copia externa es de confianza e inalterable por el atacante y se pueden compara abos archivos.

Si hay un IDS instalado en el sistema detectaría cambios en tiempo real en los ficheros del sistema. Sin embargo, si el rootkit trabaja a nivel del kernel, puede interceptar las llamadas al sistema y "cegar" al IDS devolviéndole datos falsos, por lo que la comprobación con una copia externa e inalterable sigue siendo la única prueba 100% fiable en la fase 4 (borrado de huellas).

## ✅ Parte C · Verificar una descarga: hash y firma

Descargaremos un programa pequeño y real, GNU Hello, junto a su firma y la lista de claves de GNU.

### 💡 C1. Descarga:

mkdir -p ~/verif && cd ~/verif

wget https://ftp.gnu.org/gnu/hello/hello-2.12.1.tar.gz

wget https://ftp.gnu.org/gnu/hello/hello-2.12.1.tar.gz.sig

wget https://ftp.gnu.org/gnu/gnu-keyring.gpg

<img width="828" height="70" alt="image" src="https://github.com/user-attachments/assets/64c71e33-e006-450d-a5e1-ee30f17c1a20" />


<img width="875" height="72" alt="image" src="https://github.com/user-attachments/assets/025e266a-e5c5-4141-9a17-1c7bbd82278d" />


<img width="730" height="52" alt="image" src="https://github.com/user-attachments/assets/bac8d097-2b9e-4899-bc8d-e5bd06ab4063" />

### 💡 C2. Calculad el hash:

```
📌 $ sha256sum hello-2.12.1.tar.gz
```

<img width="872" height="63" alt="image" src="https://github.com/user-attachments/assets/9fddc5fc-0b10-48f7-baa1-12e4daa80d1e" />

**Comparadlo con vuestro compañero de al lado. ¿Sale igual? Debería.**

No lo he podido comprobar, pero sí debe salir igual. Las funciones hash son deterministas, lo que significa que a partir de un mismo archivo de entrada siempre se obtiene exactamente la misma cadena hash como salida. Si el archivo analizado mi compañero y yo es idéntico, el hash será el mismo en ambos equipos.

### 💡 C3. Verificad la firma usando la lista de claves de GNU:

```
📌 $ gpg --no-default-keyring --keyring ./gnu-keyring.gpg --verify hello-2.12.1.tar.gz.sig hello-2.12.1.tar.gz
```

<img width="887" height="215" alt="image" src="https://github.com/user-attachments/assets/0ef3bf09-8224-4337-8f39-46d2d5e76c47" />

Deberíais ver Good signature from .... También saldrá un aviso WARNING: This key is not certified with a trusted signature. Es normal, significa que vosotros no habéis dicho personalmente que esa clave sea de confianza. Anotad:

* **Nombre de quien firmó:** Reuben Thomas <rrt@sc3d.org> (también identificado como keybase.io/rrt <rrt@keybase.io>)

  <img width="657" height="50" alt="image" src="https://github.com/user-attachments/assets/2a71a84a-d1ed-4245-bbaa-0c3be06a6378" />

* **Fecha de la firma:** Sun 29 May 2022 11:05:00 PM UTC
  
<img width="485" height="35" alt="image" src="https://github.com/user-attachments/assets/da02a013-0494-4b88-9907-f9b4f028d78d" />

* **Identificador de la clave:** 24093F016FFE8602EF449BB84C8EF3DA3FD37230

  <img width="565" height="35" alt="image" src="https://github.com/user-attachments/assets/2ea02769-0d8a-4a7e-b886-a76ae18ed02d" />

### 💡 C4. El experimento: ¿qué pasa si alguien toca el fichero? Haced una copia y añadid un carácter:

```
📌 $ cp hello-2.12.1.tar.gz hello-manipulado.tar.gz
📌 $ echo "x" >> hello-manipulado.tar.gz
📌 $ sha256sum hello-2.12.1.tar.gz hello-manipulado.tar.gz
📌 $ cp hello-2.12.1.tar.gz.sig hello-manipulado.tar.gz.sig
📌 $ gpg --no-default-keyring --keyring ./gnu-keyring.gpg --verify hello-manipulado.tar.gz.sig 📌 hello-manipulado.tar.gz
📌 $ gpg --no-default-keyring --keyring ./gnu-keyring.gpg --verify hello-manipulado.tar.gz.sig 📌 hello-manipulado.tar.gz
```

<img width="882" height="162" alt="image" src="https://github.com/user-attachments/assets/da53e8dc-5b45-4f42-a918-bc630f0493f8" />

<img width="887" height="125" alt="image" src="https://github.com/user-attachments/assets/496715b2-ee33-4cc7-b48e-6c783a311ab8" />


### Anotad: ¿cómo cambia el hash? ¿Qué dice ahora gpg? (Debe decir BAD signature.)

* **¿Cómo cambia el hash?:** Cambia completamente al solo añadir un caráfcter (x) al final del archivo. El hash del original es -> 8d99142afd92576f30b0cd7cb42a8dc6809998bc5d607d88761f512e26c7db20 y el del archivo modificado es -> 8c253f1ad6ea1e57b01624a938f6e81aec3685fcc53ed2e5bd5f99e22e417cf8

* **¿Qué dice ahora GPG?:** Indica bad signature, esto demuestra que la firma digital valida la integridad del fichero. Como el archivo ha sido manipulado, el hash interno recalculado no coincide con el resumen cifrado en la firma original, detectando que el archivo ha sido alterado y no es seguro.

### 💡 C5. Pregunta C: completad:


| Situación |	¿Basta con el hash?	| ¿Hace falta la firma? ¿Por qué? |
|-----------|---------------------|---------------------------------|
| El fichero se corrompió al descargarlo | Sí | No hace falta. El hash oficial publicado en la web original sirve para comprobar la integridad. Si el archivo se corrompió, el hash local no coincidirá con el oficial, advirtiendo del error |
| Un atacante cambió el fichero y el hash publicado en la misma web | No | Sí hace falta. Si el atacante tiene el control de la web, puede modificar tanto el fichero maliciosos como el hash que publica. Solo la firma digital (GPG) firmada con la clave privada del autor legítimo permitiría detectar que el hash o el archivo han sido modificados |
| Descargas desde un espejo (mirror) que no es de confianza | No | Sí hace falta. Un servidor espejo no confiable podría servir archivos modificados junto con hashes falsos. La firma digital garantiza la autenticidad e integridad extremo a extremo mediante la clave pública del creador original, independientemente de dónde se descargue el archivo | 

## ✅ Parte D · Política de actualizaciones para Torrent

### Torrent tiene tres socios y nadie se encarga del mantenimiento. Redactad una política de máximo 6 líneas que responda:

**❓ 1- ¿Quién se encarga de las actualizaciones y con qué frecuencia?**
Un socio designado actuará como responsable de IT revisando y aplicando actualizaciones mensualmente.

**❓ 2- ¿Se actualiza todo automáticamente o primero se prueba? (Pensad en el equipo del servidor de impresión y en el ERP.)**
Los equipos clientes se actualizarán automáticamente, pero el ERP y el servidor de impresión se probarán previamente antes de desplegar parches. 

**❓ 3- ¿De dónde se puede instalar software y quién lo autoriza?**
El SW solo se instalará desde repositorios oficiales previamente autorizados por el socio responsable.

**❓ 4- ¿Qué se hace con el equipo que ya no recibe parches (EOL)?**
Los equipos fuera de soporte (EOL) serán retirados o aislados de la red interna para evitar vulnerabilidades.

Para ver la opción automática en Ubuntu/Debian (solo mirad, no hace falta activarla):

```
📌 $ apt policy unattended-upgrades
📌 $ cat /etc/apt/apt.conf.d/20auto-upgrades 2>/dev/null
```

### Debate: actualizar automáticamente reduce el riesgo de que Torrent esté desactualizado, pero una actualización puede romper el ERP un viernes por la tarde. ¿Qué ganáis y qué perdéis con cada opción?

**🧠 Actualización Automática:**

Se gana más seguridad y cero carga de trabajo. Los parches de seguridad cirtical/sero-day se aplican inmediatamente sin depender del factor humano además de que los socios no tienen que estar pendientes de revisar ni aplicar parches manualmente.

Hay riesgos de interrupción del negocio y falta de control. Una actualización no probada puede romper la compatibilidad del ERP o del servidor de impresión en un momento crítico, paralizando la facturación o producción durante el fin de semana además de que no se pueden prever incompatibilidades ni cambios e configuración no deseados.

**🧠 Actualización Manual:**

Se gana estabilidad y disponibilidad además de control de tiempos. Se garantiza que el ERP y los servicios críticos funcionen correctamente antes de desplegar la actualización en producción y permite programar las actualizaciones en ventanas de mantenimiento fuera del horario laboral.

Hay exposición a ataques y requiere tiempo y esfuerzo. Mientras se prueba la actualización, el sistema permanece vulnerable a fallos de seguridad conocidos y uno de los socios debe dedicar tiempo a gestionar y probar los parches.

Windows: los mismos conceptos existen allí con otros nombres. winver (versión), Windows Update (parches), Get-FileHash fichero (hash) y Get-AuthenticodeSignature fichero (firma de un ejecutable).
