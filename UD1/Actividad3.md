## 4- Arrancar el objetivo y anotar su IP 
Para cambiar la IP en el Linux (interfaz):
Ver el nombre del archivo de la configuración de red: $ ls /etc/netplan/

<img width="376" height="25" alt="image" src="https://github.com/user-attachments/assets/f4dd8e59-cdb0-4831-a440-ba427498951f" />

Una ves se ve el nombre del archivo se tiene que abrir y modificar: $ sudo nano /etc/netplan/50-cloud-init.yaml

<img width="622" height="187" alt="image" src="https://github.com/user-attachments/assets/653559e6-12ca-4fa6-87f9-ad5c6a112605" />

Aplicar la configuración: $ sudo netplan apply

<img width="835" height="300" alt="image" src="https://github.com/user-attachments/assets/82734282-14ea-441a-8a4c-a82938ea876b" />

Con $ ip a compruebo el cambio de IP
<img width="800" height="115" alt="image" src="https://github.com/user-attachments/assets/5efff48a-5b74-4457-9e58-5d98c20ff1a1" />

Con la MV de metasplo-itable se hace lo mismo:
Con $ sudo nano /etc/network/interfaces edito el archivo

<img width="307" height="107" alt="image" src="https://github.com/user-attachments/assets/9b98d49d-6447-49e9-880d-81b4d5389a91" />

Como no me ha aparecido la IP fuerzo la IP para ese adaptador: $ sudo ip addr add 192.168.1.3/24 dev eth0 

<img width="665" height="65" alt="image" src="https://github.com/user-attachments/assets/4e958a88-592d-4787-ada3-a06fc0cf5937" />

Vuelvo a comprobar que la IP se ha cambiado correctamente: $ ip a

<img width="752" height="136" alt="image" src="https://github.com/user-attachments/assets/90e7bbe9-fd49-4dc7-8c7a-e5cefbfb3429" />

## 5- Lanzar el escaneo 

<img width="346" height="107" alt="image" src="https://github.com/user-attachments/assets/bf52ee80-ec3e-41b7-a52e-0c24ff51b845" />


<img width="695" height="87" alt="image" src="https://github.com/user-attachments/assets/18ac00b7-ca70-4e3d-9a9d-381062787ebe" />

## 6- Leer el informe
**Severidad crítica (9-10)**: Hay 6 vulnerabilidades, dentro de esta categoría hay 2 con una puntuación 10 que son: General y Gain a shell remotely (obtener un shell de forma remota).

**Severidad alta (7-8.9)**: Hay 3 vulnerabilidades las cuales todas son de 7.5 y son: RPC, Service Detecion (Detección de servicios) y General.

**Severidad media (4-6.9)**: Hay 8 vulnerabilidades, 2 de ellas con 6.5 y son: Service Detection y Misc. 

**Severidad baja (0.1-3.9)**: Hay 2, la mas alta con un 2.6 y es Service Detection.

**Iformativa (0)**: Hay 47 de tipo informativo.

## 7- Clasificar tres vulnerabilidades

| Vulnerabilidad | Severidad/CVSS | Origen (diseño, implementación, uso) | Breve descripción|
-----------------|----------------|--------------------------------------|------------------|
| VNC Server 'password' password | Crítica / 10 | Uso / Implementación | La persona que configuró el servidor VNC puso una contraseña débil, lo que hace que cualquier persona pueda acceder a el servidor de forma remota además que es cualquier persona sin demasiado conocimiento puede acceder a ella |
| rlogin Service Detection | Alta / 7.5 | Diseño | Los datos que se transmiten ente cliente y servidor pueden ser interceptados por un tercero (man-in-the-middle), ya que los datos se transmiten en texto plano. El servidor tiene una mala configuración en la autenticación es posible conseguir la numeración TCP o la suplantación de IP e incluso poder eludir la autenticación |
| TLS Version 1.0 Protocol Detection | Media / 6.5 | Diseño | El servicio remoto tiene una  serie de fallos de diseño criptográfico |

## 8- De las tres anteriores (o de otra que te llame la atención), elige una crítica o alta y documenta con más detalle:
**VNC Server 'password' password (10)**

**Qué es:** Servidor VNC con una contraseña débil (fácil de adivinar).

**Cómo se explota:** Cualquier persona que tenga el mínimo conocimiento puede acceder de forma remota y al tener una contraseña débil la adivinarían enseguida (admin, 1234, etc).

**Cómo se mitiga:** Poniendo al servidor VNC una contraseña robusta.

**Referencia:** ID 61708
