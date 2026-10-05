Para apagar la máquina se una el comando $ sudo shutdown -h now

# Parte A — Fase 1: reconocimiento
## A1. whois y nslookup desde dos máquinas



# Parte B — Fase 2: exploración y contramedidas
## B1. Preparación



## B2. Medición ANTES




## B3. Contramedidas (en Torrent-Vulnerable, con sudo

1- Hacer un nmap (ver que puertos están abiertos) desde la máquina linux con el comando $ nmap -sV 192.168.1.101 -oN antes.txt

2- Hacer un $ sudo netstat -tulpn en metasploitable para ver todos los servicios que tengan el puerto abierto

3- 

<img width="942" height="47" alt="image" src="https://github.com/user-attachments/assets/ddc9fcc3-9007-41a3-8536-cafe0d460836" />

Para parar el servicio he hecho: 

<img width="977" height="372" alt="image" src="https://github.com/user-attachments/assets/41e72f71-407d-4dfc-b868-c8cf5bc8cfba" />



## B4. Medición DESPUÉS





## B5. Preguntas finales
### 1- ¿Qué contramedida ha reducido más lo que ve el atacante? ¿Por qué?


### 2- Si solo ocultáis un banner pero el servicio sigue activo, ¿la vulnerabilidad sigue ahí? Razonad la respuesta



### 3- Torrent-Vulnerable usa un sistema operativo sin soporte desde hace años. ¿Puede una contramedida de estas sustituir a actualizarlo? ¿Qué haríais en una empresa real?


### 4- De todas las contramedidas de las partes A y B, ¿cuáles evitan que el atacante encuentre información y cuáles solo ayudan a detectar que lo están intentando?

