## Parte B · ¿De dónde vienen los programas instalados?
### B1. Mirad de qué repositorios se descarga vuestro sistema:

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
**Significado de la tabla:**

