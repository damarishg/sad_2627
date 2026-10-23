Para apagar la máquina se una el comando $ sudo shutdown -h now

# Parte B — Fase 2: exploración y contramedidas
## B1. Preparación
1. Arrancad Torrent-Auditor y Torrent-Vulnerable.
2. Antes de tocar nada, haced una instantánea de Torrent-Vulnerable (en VirtualBox: menú Máquina → Tomar instantánea). Vais a modificarla y luego la necesitaremos tal como estaba.
3. Anotad la IP de Torrent-Vulnerable (ip a dentro de ella).
4. Desde Torrent-Auditor, comprobad que la veis: ping -c 3 <IP>.


## B2. Medición ANTES

Al hacer el ejercicio 3 primero no tengo capturas del comando en el Auditor. Pero salían 21 puertos abiertos desde la máquina auditora.

## B3. Contramedidas (en Torrent-Vulnerable, con sudo)

### CONTRAMEDIDA 1 

### COMANDO FIND
También se puede encontrar un servicio con el siguiente comando 
```
📌$ sudo find / -name "*nombre_servicio*" 2>/dev/null
```
### :pushpin: ¿Cuándo usar 'ls' o 'grep'?
Depende de si vas a consultar una **carpeta** o un **archivo de texto** (configuración).
1. Usar 'ls' cuando buscas dentro de una **carpeta** donde cada servicio es un archivo independiente
2. Usar 'grep -n' cuando buscas dentro de un **archivo de configuración**.

**💡 Como usar 'ls' correctamente**
```
📌 $ ls -l /etc/carpeta | grep nombre_servicio
```

**💡 También puedes buscar dentro de todas las carpetas a la vez:**
```
📌 $ sudo ls -l /etc/init.d/ /etc/xinetd.d/ /etc/rc2.d/ | grep nombre_servicio
```
**💡 Como usar 'grep -n' correctamente**
```
📌 $ sudo grep -n nombre_servicio /etc/archivo_de_confi
```
**💡 Atajo para buscar dentro de todo el sistema**
```
📌 $ sudo grep -rn "nombre_de_confi" /etc/ (-r busca de forma recursiva en las carpetas y -n muestra el archivo u nº de línea exacto)
```
### 📌 Como saber si es el servicio es una carpeta, archivo de texto o script
1. Si al hacer ls -l aparece la d es un directorio
2. Si aparece rw es un archivo de texto
3. Si aparece al menos una x (aunque también estén rw) es un script

### 🔄 ¿Cómo recargar los cambios según el archivo modificado?
* **HACER PRIMERO EL ÚLTIMO PUNTO**
* **Si modifico `/etc/inetd.conf` (INETD):**
 ```
 📌 sudo /etc/init.d/openbsd-inetd restart
```

* **Si modifico un archivo en '/etc/xinetd.d/' (XINETD):**
```
📌 sudo /etc/init.d/xinetd reload
```

### ⚙️ Servicios lanzados desde `/etc/rc.local` ( Por ejemplo: rmiregistry / Puerto 1099)
Si el servicio no aparece en `/etc/initd.d/`, `inetd.conf`, `/etc/xinetd.d/`, `xinetd.conf`, `/etc/rc2.d/` o alguna otra ruta mencionada en la guía lo más probable es que se se arranque al inicio desde `/etc/rc.local`

**PASOS PARA DETENER EL SERVICIO**
1. Matar el proceso activo en memoria:
```
📌 sudo killall nombre_servicio # Ejemplo: sudo killall rmiregistry
```
2. Editar el archivo de rc.local poniendo # delante de la línea del servicio

---------------------------------------------------------------------------------------------------
### SERVICIO EJECUTADO POR XINETD (ARCHIVO)
**1-** Hacer un nmap (ver que puertos están abiertos) desde la máquina linux con el comando 
```
📌 $ nmap -sV 192.168.1.101 -oN antes.txt
```
**Conectarse con el PUTTY poniendo la IP 192.168.56.2**

**2-** Hacer un :pushpin:`$ sudo netstat -tulpn` en *metasploitable* para ver todos los servicios que tengan el puerto abierto


**3-** Escoger un servicio, en mi caso el del puerto 513 y mirar dentro de */etc/services* con el comando
```
📌 $ grep  513 /etc/services
```
<img width="510" height="92" alt="image" src="https://github.com/user-attachments/assets/a4039ecb-1345-4476-9c9f-bb20a54075eb" />


**4-**  Ahora al encontrar el nombre del proceso (en este caso login ) tengo que buscar dentro de */etc/inetd.d* y dentro de */etc/xinetd.d* con el comando 
```
📌 $ sudo grep -n -w login /etc/
```
<img width="863" height="66" alt="image" src="https://github.com/user-attachments/assets/515af8cd-58fa-4587-b2f7-2034abeab2f6" />


**5-** Modificar el archivo */etc/inetd.conf* poniendo una # delante para que no arranque el servicio

<img width="315" height="40" alt="image" src="https://github.com/user-attachments/assets/07b924d1-3588-4032-af62-27e24d124d00" />


**6-** Recargar el archivo modificado anteriormente con el comando:
```
 📌 $ sudo /etc/init.d/xinetd reload
```
<img width="605" height="42" alt="image" src="https://github.com/user-attachments/assets/ef371fcd-9f84-4acb-baa3-73bd0be4bf63" />

**7-** Comprobar que el puerto se ha cerrado con el comando  (pongo 513 porque es el puerto que estaba abierto)
```
📌 $ sudo netstat -tulpn | grep 513
```
<img width="970" height="140" alt="image" src="https://github.com/user-attachments/assets/14e4ddd2-ee99-4cf4-8bab-304ef674cf6c" />


------------------------------------------------------------------------------------------------
### SERVICIO EJECUTADO POR - (SCRIPT)

**1-** Averiguar el nombre del servicio 2049 con el comando 
```
📌 $grep 2049 /etc/services
```
<img width="737" height="101" alt="image" src="https://github.com/user-attachments/assets/2c49ad88-5162-4ec6-9923-f916af93c96e" />


**2-** Averiguar donde se encuentra el servicio. Se puede averiguar de diferentes maneras: listando, haciendo cat, etc. Hay que ir probando hasta encontrar donde se encuentra. Hay que buscar en init.d, xineted.d, inetd.conf, xineted.conf, rc2.d. Haciendo :pushpin:`$ls /etc/rc2.d/ `
En rc2.d he encontrado 2 servicios con nfs. Para encontrar la ruta se usa el comando :pushpin:`ls -l | grep nfs`

<img width="870" height="165" alt="image" src="https://github.com/user-attachments/assets/26748b24-755b-4ff6-840e-ebd41f978ead" />

<img width="562" height="75" alt="image" src="https://github.com/user-attachments/assets/2e318b7b-1bad-4100-89f0-1ee31bcdd7dc" />


<img width="942" height="86" alt="image" src="https://github.com/user-attachments/assets/0df25fc3-5b24-4833-88b8-77b17408e729" />

**3-** Al ser un script no podemos ponerle una # como los que están dentro de un .conf, entonces hay que hacer lo siguiente (hay dos NFS, las fotos es de uno, pero hay que hacerlo en los dos):
1. Detener el proceso para que no haya problemas con el comando:
```
📌 $ sudo /etc/init.d/nfs-common stop
```   
<img width="937" height="47" alt="image" src="https://github.com/user-attachments/assets/df03bc86-f722-4db6-9ecf-3cbda5fa7763" />

2. Borrar los enlaces de rc.d (no se elimina, pero al no estar en esa carpeta no se ejecuta) con el comando:
```
📌 $ sudo update-rc.d -f nfs-common remove
```
<img width="727" height="220" alt="image" src="https://github.com/user-attachments/assets/2470de98-dd5f-4b89-905d-040f60f5fff0" />

3. Comprobar que se ha eliminado de las carpetas rc.d (sale ese nfs porque aún no lo he borrado).

<img width="535" height="52" alt="image" src="https://github.com/user-attachments/assets/d3e82641-7903-44d9-bf6f-d593aa752c1c" />

4. Al ser un script hay que reiniciar la máquina con:
```
📌 $ sudo reboot
```
5. Comprobar que el puerto se ha cerrado:
```
📌 $ sudo netstat -tulpn | grep 2049
```
<img width="658" height="107" alt="image" src="https://github.com/user-attachments/assets/f91c9499-ab1d-40d0-9c52-cfaf002abd14" />

----------------------------------------------------------------------------------------------------
### Servicio con nombre jsvc (SCRIPT)

**1-** Encontrar el servicio. He hecho todo lo de las capturas y no he encontrado nada al respecto sobre ese servicio

<img width="913" height="68" alt="image" src="https://github.com/user-attachments/assets/a6f3274f-eb23-4a0f-84a6-6c3b4304bee9" />

**2-** Para poder encontrarlo he tenido que hacer `📌$ sudo grep -r jsvc /etc/` . Jsvc no es el nombre del servicio, realmente se llama tomcat5.5 y está dentro de rc2.d

<img width="702" height="253" alt="image" src="https://github.com/user-attachments/assets/922c27ce-78c1-4bdb-8b5c-7a5e6467713d" />
```
⚠️ TAMBIÉN SE PEUDE BUSCAR DIRECTAMENTE CON EL COMANDO `📌 $ sudo grep -l jsvc /etc/inetd.d o /etc/xinetd.d` en vez de buscar en /etc/
```
**3-** Hago un ls dentro de rc2.d para encontrar el servicio

<img width="836" height="76" alt="image" src="https://github.com/user-attachments/assets/b29460eb-5bee-4475-aa6c-76c1423aef85" />

**4-** Como es un ejecutable tengo que detener el proceso con el comando 
```
📌 $ sudo /etc/init.d/tomcat5.5 stop
```
<img width="631" height="46" alt="image" src="https://github.com/user-attachments/assets/73c9f841-bad2-4bdc-8cf3-91399a9ad549" />

**5-** Una vez detenido lo elimino de las carpetas rc para que no se ejecute al arrancar con el comando 
```
📌 $ sudo update-rc.d -f tomcat5.5 remove
```
<img width="687" height="50" alt="image" src="https://github.com/user-attachments/assets/e4194c71-9478-4788-bd86-def4a1357f8f" />

**6-** Comprobar que el servicio se ha parado y reiniciar la máquina:

<img width="616" height="57" alt="image" src="https://github.com/user-attachments/assets/6316485c-5cad-4d3d-b9de-4af351c89de4" />

------------------------------------------------------------------------------------------------
SERVICIO CON NOMBRE PROPIO (MYSQL) mysqld

**1-** EN el netstat sale que se llama mysqld, pero al hacer `📌 $ grep 3306 /etc/service` sale el nombre real del servicio.

**2-** Para encontrarlo hacemos lo siguiente:
```
📌 $ sudo ls -l /etc/init.d/ /etc/xinetd.d/ /etc/rc2.d/ | grep mysql
```
<img width="941" height="165" alt="image" src="https://github.com/user-attachments/assets/9c194ceb-2923-4bf7-9003-32978e4df746" />

**3-** En la imagen anterior aparecen (hay 3 mysql) que está dentro de init.d y es un script, entonces hay que hacer lo siguiente con cada uno de ellos:

1. Parar el servicio
```
📌 $ sudo /etc/init.d/mysql stop
```

<img width="951" height="62" alt="image" src="https://github.com/user-attachments/assets/2112ec04-1d43-436a-bef8-e526270908f9" />

2. Eliminarlo de rc.d

<img width="642" height="65" alt="image" src="https://github.com/user-attachments/assets/700e77ac-8eb8-4009-8533-8cca9839f9b7" />

3. Comprobar que se ha cerrado y hacer reboot a la máquina:
   
<img width="671" height="61" alt="image" src="https://github.com/user-attachments/assets/8117df81-af98-4dd0-b8f8-94d70fa9ab5f" />


### PUERTOS ABIERTOS EN EL SISTEMA
No sabía si solo se cerraban las 0.0.0.0 o todas, veía raro cerrar algunos servicios como el DNS, pero como no estaba segura solo he dejado el puerto 80 (apache2), el 22 (ssh) y el UDP.

<img width="960" height="203" alt="image" src="https://github.com/user-attachments/assets/006bfe1a-69a2-4c68-b6f9-f465977b8d97" />

### CONTRAMEDIDA 2

### 📌 BLOQUEAR EL PING

* Desde la máquina Vsudosdulnerable:
```
📌 $ sudo iptables -A INPUT -p icmp --icmp-type echo-request -j DROP
```
<img width="963" height="67" alt="image" src="https://github.com/user-attachments/assets/ef403932-16d8-4fee-937a-7f62073217a8" />

* Desde la máquina Auditora:
```
📌$ ping -c 3 192.168.1.101 
```

<img width="688" height="142" alt="image" src="https://github.com/user-attachments/assets/569e63ae-abbf-4e0f-80c2-3ae6ca1d79be" />

Al bloquearse las solicitudes de echo-request los paquetes enviados por el Auditor se han perdido.

## B4. Medición DESPUÉS

En Torrent-Auditor:
```
📌 $ sudo nmap -sV <IP> -oN despues.txt
📌 $ diff antes.txt despues.txt
```
<img width="672" height="73" alt="image" src="https://github.com/user-attachments/assets/a30b5e67-78b6-40ac-8a42-324d11e62351" />


Rellenad la tabla:

| Aspecto | Antes | Contramedida aplicada | Después |
|---------|-------|-----------------------|---------|
| Puertos abiertos | 21 | Cierre de servicios innecesarios y desvinculación del arranque | 2 (ssh 22 y apache2 8)
| Servicios / versiones visibles | vsftpd 2.3.4, ISC BIND 9.4.2, PostgreSQL, TomCAt, etc | Desactivación y detención de los no esenciales | OpenSSH 4.7p1 y Apache httpd 2.2.8 |
| Respuesta al ping | Responde a ping | Regla de corta fuego en la MV vulnerable | Paquetes sin respuesta |

## B5. Preguntas finales
### 1- ¿Qué contramedida ha reducido más lo que ve el atacante? ¿Por qué?

La contramedida que más ha reducido lo que ve el atacante es la desactivación  y cierre de servicios innecesarios (puertos). Al eliminar esos servicios minimizamos drásticamente la superficie de ataque visible ante un escaneo de puertos. Aunque al tratarse de un SOp obsoleto, el riesgo cero no existe y requiere otras medidas adicionales de defensa.

### 2- Si solo ocultáis un banner pero el servicio sigue activo, ¿la vu*lnerabilidad sigue ahí? Razonad la respuesta

Sí, porque aun que no se ve la vulnerabildiad sigue existiendo. Ocultar o modiifcar el banner no elimina la vulnerabilidad. 
Aunque a simple vista no se muestre, el SW vulnerable sigue ejecutándose en el puerto y un atacante puede identificarla.


### 3- Torrent-Vulnerable usa un sistema operativo sin soporte desde hace años. ¿Puede una contramedida de estas sustituir a actualizarlo? ¿Qué haríais en una empresa real?

No, una contramedida no puede sustituir un soporte. Puedes hacer cosas para que sea un poco más difícil que un atacante entre al equipo, pero al no tener actualizaciones siempre puede quedar una puerta abierta.
En una empresa real no usaría un SOp obsoleto, buscaría uno con soporte para mantener la seguridad y confianza de la emrpesa.

### 4- De todas las contramedidas de las partes A y B, ¿cuáles evitan que el atacante encuentre información y cuáles solo ayudan a detectar que lo están intentando?

* Contramedidas que evitan que el atacante encuentre información: Cierre y desactivación de servicios innecesarios, bloqueo de las solicitudes eco, no poner información para que no salga en el whois.
* Contramedidas que solo ayudan a detectar que lo están intentando: Registrar los escaneos con el cortafuegos con la regla iptables -j LOG permite monitorizar y almacenar los registros del kernel.
