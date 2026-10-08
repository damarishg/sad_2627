Para apagar la máquina se una el comando $ sudo shutdown -h now

# Parte A — Fase 1: reconocimiento
## A1. whois y nslookup desde dos máquinas



# Parte B — Fase 2: exploración y contramedidas
## B1. Preparación



## B2. Medición ANTES




## B3. Contramedidas (en Torrent-Vulnerable, con sudo

### XINETD
**1-** Hacer un nmap (ver que puertos están abiertos) desde la máquina linux con el comando $ nmap -sV 192.168.1.101 -oN antes.txt


**2-** Hacer un $ sudo netstat -tulpn en metasploitable para ver todos los servicios que tengan el puerto abierto


**3-** Escoger un servicio, en mi caso el del puerto 513 y mirar dentro de /etc/services con el comando $ grep  513 /etc/services

<img width="510" height="92" alt="image" src="https://github.com/user-attachments/assets/a4039ecb-1345-4476-9c9f-bb20a54075eb" />


**4-**  Ahora al encontrar el nombre del proceso tengo que buscar dentro de /etc/inetd.d y dentro de /etc/xinetd.d con el comando $ sudo grep -n -w login /etc/

<img width="863" height="66" alt="image" src="https://github.com/user-attachments/assets/515af8cd-58fa-4587-b2f7-2034abeab2f6" />


**5-** Modificar el archivo /etc/inetd.conf poniendo una # delante para que no arranque el servicio

<img width="315" height="40" alt="image" src="https://github.com/user-attachments/assets/07b924d1-3588-4032-af62-27e24d124d00" />


**6-** Recargar el archivo modificado anteriormente con el comando $ sudo /etc/init.d/xineted reload   (siempre poner el mismo path si se modifica inet.d o xinetd)

<img width="605" height="42" alt="image" src="https://github.com/user-attachments/assets/ef371fcd-9f84-4acb-baa3-73bd0be4bf63" />



**7-** Comprobar que el puerto se ha cerrado con el comando $ sudo netstat -tulpn | grep 513 

<img width="970" height="140" alt="image" src="https://github.com/user-attachments/assets/14e4ddd2-ee99-4cf4-8bab-304ef674cf6c" />


--------------------------------------------------------------------------------------------------
**1-** Averiguar el nombre del servicio 2049 con el comando $grep 2049 /etc/services 

<img width="737" height="101" alt="image" src="https://github.com/user-attachments/assets/2c49ad88-5162-4ec6-9923-f916af93c96e" />


**2-** Averiguar donde se encuentra el servicio. Se puede averiguar de diferentes maneras: listando, haciendo cat, etc: Hay que ir probando hasta encontrar donde se encuentra. Hay que buscar en init.d, xineted.d, inetd.conf, xineted.conf, rc2.d. En rc2.d he encontrado 2 servicios con nfs.

<img width="870" height="165" alt="image" src="https://github.com/user-attachments/assets/26748b24-755b-4ff6-840e-ebd41f978ead" />

<img width="562" height="75" alt="image" src="https://github.com/user-attachments/assets/2e318b7b-1bad-4100-89f0-1ee31bcdd7dc" />


<img width="942" height="86" alt="image" src="https://github.com/user-attachments/assets/0df25fc3-5b24-4833-88b8-77b17408e729" />

**3-** 














## B4. Medición DESPUÉS





## B5. Preguntas finales
### 1- ¿Qué contramedida ha reducido más lo que ve el atacante? ¿Por qué?


### 2- Si solo ocultáis un banner pero el servicio sigue activo, ¿la vulnerabilidad sigue ahí? Razonad la respuesta



### 3- Torrent-Vulnerable usa un sistema operativo sin soporte desde hace años. ¿Puede una contramedida de estas sustituir a actualizarlo? ¿Qué haríais en una empresa real?


### 4- De todas las contramedidas de las partes A y B, ¿cuáles evitan que el atacante encuentre información y cuáles solo ayudan a detectar que lo están intentando?

