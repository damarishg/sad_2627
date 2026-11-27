# Parte 1 · Montar la CA con Easy-RSA y emitir los certificados
## 1.1 Instalar Easy-RSA y preparar la PKI de la CA

Paso 1: Actualizar la MV 

Paso 2: Instalar Easr-RSA

<img width="393" height="40" alt="image" src="https://github.com/user-attachments/assets/4b241ab9-b346-4127-96fc-27296ac5acc6" />

<img width="375" height="35" alt="image" src="https://github.com/user-attachments/assets/05d61755-d7eb-44bb-a76b-bebe35f858b8" />

Comprobación versión:
<img width="413" height="72" alt="image" src="https://github.com/user-attachments/assets/1302c630-d9be-4975-9700-811f74c9e574" />

Inicialización de la PKI:
<img width="510" height="125" alt="image" src="https://github.com/user-attachments/assets/5a562199-5385-441a-b0e2-b093bc9601e1" />

## 1.2 Crear la CA

1234 CA

<img width="426" height="90" alt="image" src="https://github.com/user-attachments/assets/d9636a4e-efb5-4298-a444-63aa99ddb365" />

Common name:
<img width="757" height="56" alt="image" src="https://github.com/user-attachments/assets/3a4e7701-d443-40a5-9557-af5041417f6a" />

Comprobación de la PKI:
<img width="955" height="148" alt="image" src="https://github.com/user-attachments/assets/03b50bf4-855c-47fb-aca0-73e48b2325bc" />

<img width="576" height="288" alt="image" src="https://github.com/user-attachments/assets/a63b6427-50d3-406a-b50a-a2fa3ce27521" />

## 1.3 Certificado de la pasarela (servidor)
### Paso A · En dmz-vpn: generar la clave y la petición. 

Paso 1: Actualizar e instalar Easy-RSA / OpenVPN / nftables

<img width="543" height="47" alt="image" src="https://github.com/user-attachments/assets/5a0a7d3a-4a5f-43ec-9f70-cba5a4666ade" />

Paso 2: Crear la carpeta de Easy-RSA y la PKI (ihual que en el srv-ca)

Paso 3: Generar la clave privada de la pasarela vpn y la petición de la firma del ceritifcado (CSR) sin contraseña (nopass)

<img width="502" height="37" alt="image" src="https://github.com/user-attachments/assets/f8ddf686-15c0-4b99-9b5f-80b4850f7e68" />

<img width="698" height="42" alt="image" src="https://github.com/user-attachments/assets/a2f55b1f-755d-43e0-8583-f545b987cc1f" />

### Paso B · Llevar la petición a srv-ca.
Paso 1: Copiar la petición desde dmz-vpn hasta srv-ca:

<img width="640" height="41" alt="image" src="https://github.com/user-attachments/assets/e380d546-cc93-410e-96be-2087b0ae8451" />

Paso 2: Comprobar que la petición está en srv-ca:

<img width="441" height="38" alt="image" src="https://github.com/user-attachments/assets/d3c0bcd9-da41-4cae-811b-50d956748f0e" />

### Paso C · En srv-ca: importar y firmar.
Paso 1: Importar la solicitud:

<img width="780" height="182" alt="image" src="https://github.com/user-attachments/assets/21f5f722-9a01-47df-bcac-002a30e95d61" />

Paso 2: Firmar certificado:

<img width="982" height="48" alt="image" src="https://github.com/user-attachments/assets/da5c86f0-960c-4db9-903a-37d133645e2b" />

<img width="418" height="90" alt="image" src="https://github.com/user-attachments/assets/73700c0b-e5b3-495f-966a-045fa6727843" />

Comprobación:
<img width="961" height="95" alt="image" src="https://github.com/user-attachments/assets/cd9be1b5-e4b8-4d72-bd63-b10804104d16" />

## 1.4 Certificado del cliente (el técnico)
### Paso A · En el cliente: generar clave y petición.

Hacer lo mismo que en el DMZ-VPN

## Paso B · Llevar la petición a srv-ca. Igual que en 1.3.

Para poder importar el certificado desde el Cliente a SRV-CA hay que usar a DMZ-VPN como intermediario.

Desde el cliente:


