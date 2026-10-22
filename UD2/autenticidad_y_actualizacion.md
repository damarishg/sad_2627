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

**Anotad: ¿cuántos paquetes tienen actualización pendiente?**




### 💡 A3. Simulad la actualización sin aplicarla:

```
📌 $ sudo apt-get -s upgrade |head -30
```
La opción -s es "simular". Fijaos en que dice qué instalaría, pero no toca nada.


### 💡 A4. ¿Y si el sistema ya no recibe parches? Buscad en https://endoflife.date la fecha de fin de soporte de:
* La versión de Ubuntu/Debian de vuestro Torrent-Auditor.
* Ubuntu 8.04, la que tiene Torrent-Vulnerable (Metasploitable2).

Rellenad:

| Sistema | Fecha de fin de soporte | ¿Recibe parches hoy? |
|---------|-------------------------|----------------------|
| Torrent-Auditor | 
| Torrent-Vulnerable (Ubuntu 8.04) | 09 de mayo de 2013 | No |

### Pregunta A: En la oferta de empleo de Torrent (dossier de la UD02) decían que trabajaban con "Ubuntu 8.04". Aunque pusieran todas las contraseñas bien, ¿qué problema sigue sin tener solución?

El SOp no tiene soporte ni actualizaciones, tener un SOp con estas características es un error de seguridad muy grave. Como he dicho no tiene soporte y al no tener actualizaciones no tiene parches. Pueden usar Exploits para acceder al sistema sin ningún esfuerzo ni mucho conocimiento.

## ✅ Parte B · ¿De dónde vienen los programas instalados?
### B1. Mirad de qué repositorios se descarga vuestro sistema:

```
📌 $ cat /etc/apt/sources.list

📌 $ ls /etc/apt/sources.list.d/
```
 
Investiga el fichero donde están configurados los repositorios y comenta el significado de cada una de las líneas y su contenido.

<img width="655" height="270" alt="image" src="https://github.com/user-attachments/assets/d73a6fb5-1dd0-4b03-9830-9c7a3a97b759" />

<img width="481" height="57" alt="image" src="https://github.com/user-attachments/assets/0e6e1a34-82cb-4c8a-a71d-91cac664ed20" />

**1- Types:** Indica el tipo de repositorio. DEB significa que descargará paquetes precompilados e instalables.

**2- URIs :** Es la dirección web  (servidor espejo) desde donde se descargan los programas.

**3- Suites:** Es la versión específica de Ubuntu y las ramas de paquetes que va a buscar. NOBLE: Es el nombre en clave de la versión de la versión de Ubuntu.

NOBLE-UPDTES: Actualizaciones y correcciones que se lanzan tras la salida del sistema. 


NOBLE-BACKPORTS: Versiones más recientes de ciertos programas adaptadas para esta versión.

NOBLE-SECURITY: Parches de seguridad de seguridad específicos.

**4- Components:** Categorías o secciones de SW que tienes activas según sus licencias y soporte. MAIN: SW libre y de Código Abierto respaldado con soporte oficial de Canonical.

RESTRICTED: Controladores propietarios del sistema.

UNIVERSE: Programas libres mantenidos por la comunidad de Linux.

MULTIVERSE: Programas no libres y restringidos por patentes o derechos de autor.

**5- Signed-By:** Clave digital de Seguridad (clave pública de Ubuntu).

### B2. apt solo instala paquetes firmados por claves en las que confía. Mirad dónde están esas claves

<img width="812" height="292" alt="image" src="https://github.com/user-attachments/assets/f8f976fb-c3b1-4814-b2fa-1ad662667480" />

**Reflexiona: ¿qué pasaría si un atacante introduce una clave falsa en /usr/share/keyrings/? 
¿y si intercepta o reemplaza la clave oficial de Ubuntu ubuntu-archive-keyrings.gpg?**
En el caso de que introduzca un a clave y tú descargues un paquete que tenga como clave la que ha puesto el atacante lo más probable es que estén instalando un SW con 
un maleware o algo que el atacante quiere que tu PC tenga.
En el caso de que intercepte la clave oficial sería peor, ya que no hace falta descargar un SW específico, si no que con cualquier SW tú PC está en peligro.

### B3. Información de un paquete concreto. Elegid openssh-server o curl:

<img width="816" height="277" alt="image" src="https://github.com/user-attachments/assets/c96321cf-8550-4299-841f-f3122b9832bb" />

Anotad: versión instalada, versión candidata, repositorio de origen y el significado de la tabla de versiones.

**Versión instalda:** 1:9.6p1-3ubuntu13.19

**Versión candidata:** 1:9.6p1-3ubuntu13.19

**Repositorio de origen:** 

<img width="556" height="52" alt="image" src="https://github.com/user-attachments/assets/c1df3565-58c0-4d92-bc23-6225777f8a08" />


**Significado de la tabla:**  Los * indica la versión instalada actualmente en el sistema. Los números indican una prioridad, a mayor número mayor prioriad (500, 100). Primero mira en el nº 500 en el caso de que falle mira en el nº 100. 

/var/lib/dpkg/status: Es la BBDD local del sistema que confirma que el paquete ya se encuentra instalado físicamente.


### B4. Buscad paquetes instalados que no proceden de ningún repositorio (instalados a mano):

<img width="896" height="187" alt="image" src="https://github.com/user-attachments/assets/b28f8d7a-2c53-42ce-ac08-d2019a3ca847" />

Cada línea que salga merece una pregunta: ¿quién lo instaló, de dónde, y quién lo verificó?

**📌 Nessus**

Es el único que está marcado como [installed. local]

**¿Quién lo instaló?:** El administrador del sistema de forma manual.

**¿De dónde?:** Desde un archivo instalador local (.deb) descargado externamente (desde Internet) y no desde los repositorios oficiales de Ubuntu.

**¿Quién lo verificó?:** El propio administrador al descargarlo del fabricante. Al no ser un paquete de repositorio, el SOp no pudo  verificarlo de forma automática mediante las claves de confianza de Ubuntu.

**📌 Resto de paquetes**

**¿Quién lo instaló?:** El gestor de paquetes del sistema (apt) de forma automática al resolver dependencias requeridas por otras aplicaciones.

**¿De dónde?:** Proceden directamente de los repositorios oficiales de Ubuntu ( como noble, noble-updates o noble-security).

**¿Quién lo verificó?:** El SOp Ubuntu mediante verificación automática de firmas digitales y claves de autenticidad del repositorio oficial.

### B5. Comprobad si algún fichero de un paquete ha cambiado desde que se instaló:

sudo dpkg -V

Si no sale nada, todo coincide con lo instalado.

Si salen líneas, la primera columna indica qué cambió (5 = el hash MD5 es distinto). Los ficheros de configuración (c) cambian de forma normal. Un binario (/usr/bin/...) modificado es una alarma

<img width="725" height="208" alt="image" src="https://github.com/user-attachments/assets/a7adb524-ce9d-487d-89bf-52f444a3aa0d" />

Pregunta B: dpkg -V ¿detectaría un rootkit que además hubiera modificado la base de datos de dpkg? ¿Por qué conviene comparar con una copia guardada fuera del sistema? (Pista: recordad la fase 4 de la intrusión.)

La cuarta fase es la de borrar huellas, en este caso al modificar también la BBD de dpkg no ve ninguna modificación, por ende el rootkit no sería detectado. Conviene tener una copia fuera porque en el caso de duda de modificación la copia externa es de confianza e inalterable por el atacante y se pueden compara abos archivos.


## Parte C · Verificar una descarga: hash y firma

Descargaremos un programa pequeño y real, GNU Hello, junto a su firma y la lista de claves de GNU.

### C1. Descarga:

mkdir -p ~/verif && cd ~/verif

wget https://ftp.gnu.org/gnu/hello/hello-2.12.1.tar.gz

wget https://ftp.gnu.org/gnu/hello/hello-2.12.1.tar.gz.sig

wget https://ftp.gnu.org/gnu/gnu-keyring.gpg


### C2. Calculad el hash:

sha256sum hello-2.12.1.tar.gz




