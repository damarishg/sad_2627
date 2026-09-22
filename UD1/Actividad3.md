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

Con la MV de metasploitable se hace lo mismo:
Con $ sudo nano /etc/network/interfaces edito el archivo

<img width="307" height="107" alt="image" src="https://github.com/user-attachments/assets/9b98d49d-6447-49e9-880d-81b4d5389a91" />

Como no me ha aparecido la IP fuerzo la IP para ese adaptador: $ sudo ip addr add 192.168.1.3/24 dev eth0 

<img width="665" height="65" alt="image" src="https://github.com/user-attachments/assets/4e958a88-592d-4787-ada3-a06fc0cf5937" />

Vuelvo a comprobar que la IP se ha cambiado correctamente: $ ip a

<img width="752" height="136" alt="image" src="https://github.com/user-attachments/assets/90e7bbe9-fd49-4dc7-8c7a-e5cefbfb3429" />

## 5- Lanzar el escaneo 

<img width="346" height="107" alt="image" src="https://github.com/user-attachments/assets/bf52ee80-ec3e-41b7-a52e-0c24ff51b845" />




| Vulnerabilidad | 
