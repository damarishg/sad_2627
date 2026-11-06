# Configuración de red 
He elegido NAT y Adaptador Puente

Esta es la configuración de red:

<img width="277" height="146" alt="image" src="https://github.com/user-attachments/assets/5be02da0-d899-4446-8267-00df49e6710a" />

#  PARTE 1 Nuestra propia autoridad de certificación
## 1.1 Cómo funciona
## 1.2 Paso a paso
**Paso 0 — Instalar Apache y comprobar que funciona (todavía sin cifrar)**
```
📌 $ sudo apt update
📌 $ sudo apt install -y apache2 openssl curl cron
📌 $ systemctl status apache2 --no-pager
📌 $ sudo ss -tlnp | grep ':80'
```

<img width="822" height="173" alt="image" src="https://github.com/user-attachments/assets/f57a8cca-8d8f-47e8-8623-4c5d60c8e257" />

<img width="807" height="76" alt="image" src="https://github.com/user-attachments/assets/a77415bd-3269-47c0-9c7c-a3aea48cae73" />

Una captura del navegador con la barra de direcciones. ¿Qué indica el navegador sobre la seguridad de la conexión y por qué?

El navegador indica que la conexión es segura si aparece le candado junto a la dirección de la página. Esto significa que usa HTTPS y que la información que se intercambia entre el navegador y la página web está cifrada, evitando que otras personas puedan leerla fácilmente.

**Paso 1 — Crear una carpeta de trabajo para la CA**
```
📌 $ mkdir -p ~/ca/private ~/ca/certs
📌 $ chmod 700 ~/ca/private
📌 $ cd ~/ca
```

<img width="536" height="142" alt="image" src="https://github.com/user-attachments/assets/e87f8efd-26de-4f41-bfbe-561aff57dee2" />

**Paso 2 — Generar la clave privada de la CA**
```
📌 $ openssl genrsa -aes256 -out private/ca.key 4096
```

**openssl:** Herramienta que usamos para trabajar con certificados y claves.
**genrsa:** Genera una calve privada RSA.
**-aes256:** Protege la clave privada mediante cifrado y una contraseña.
**-out private/ca.key:** Guarda la calve en el archivo *ca-key*, dentro de la carpeta *private*.
**4096:** Indica el tamaño de la clave en bits. Cuanto mayor sea, más difícil resulta atacarla por fuerza bruta.

<img width="717" height="83" alt="image" src="https://github.com/user-attachments/assets/78899468-279f-4e13-b237-a175ecb69fee" />

¿Por qué conviene proteger con contraseña la clave privada de una CA aunque esté en un
directorio con permisos restrictivos?

Porque aunque el directorio tenga permisos restrictivos, alguien podría conseguir copiar el archivo de la clave privada. Si está protegida con contraseña, no podrá usarla sin conocerla.

**Paso 3 — Crear el certificado raíz de la CA (autofirmado)**
```
📌 enssl req -x509 -new -key private/ca.key -sha256 -days 3650 \
 -out certs/ca.crt \
 -subj "/C=ES/O=Torrent S.L./CN=Torrent S.L. CA Raiz" \
 -addext "basicConstraints=critical,CA:TRUE" \
 -addext "keyUsage=critical,keyCertSign,cRLSign"
```

**req -x509:** crea directamente un certificado (sin esta opción crearía solo una petición de certificado).
**-days 3650:** validez de 10 años. Las raíces duran mucho; los certificados de servidor, poco.
**-subj:** los datos de identidad (país, organización y nombre de la CA).
**basicConstraints CA:TRUE:** declara «este certificado pertenece a una CA y puede firmar otros».
**keyUsage keyCertSign:** limita para qué puede usarse la clave: firmar certificados.

<img width="825" height="165" alt="image" src="https://github.com/user-attachments/assets/9841d482-8c8d-440f-94b1-1913180d67e2" />

***Paso 4 — Inspeccionar el certificado que acabas de crear**

Un certificado es un documento legible. Comprueba que contiene lo que esperas.
```
📌 $ openssl x509 -in certs/ca.crt -noout -subject -issuer -dates
📌 $ openssl x509 -in certs/ca.crt -noout -text | grep -A1 'Basic Constraints'
```

<img width="830" height="165" alt="image" src="https://github.com/user-attachments/assets/73a406bf-dc80-46ef-b6d3-2452a8bd8d0a" />

**openssl x509:** Permite consultar información de un certificado.
**-in certs/ca.crt:** Indica qué certificado queremos consultar-
**-noout:** Evita mostrar el certificado completo.
**-subject:** Muestra quién es el titular del certificado (nuestra CA)-
**-issuer:** Muestra quién ha firmado el certificado.
**-dates:** Muestra la fecha de inicio y fin de su validez.

<img width="830" height="96" alt="image" src="https://github.com/user-attachments/assets/05865e11-f104-48f9-b547-455fe1431f33" />

**-text:** Muestra toda la información del certificado.
**-A1:** Muestra la línea siguiente a la coincidencia.
**'Basic Constraints':** Busca la extensión que indica si el certificado puede actuar como CA.

**Critical:** Es muy importante. Si un programa no la entiende, debe rechazar el certificado.
**CA:TRUE:** Indica que el certificado pertenece a una Autoridad de Certificación y puede usarse para firmar otros certificados.

¿Por qué coinciden Subject e Issuer? 

Porque es un certificado autofirmado. Es decir, nuestra propia CA (Torrent S.L) se ha firmado su propio certificado, por eso el titular (Subject) y el emisor (Issuer) son la misma entidad.

¿Hasta qué fecha es válida tu CA?

Es válida hasta el 3 de Noviembre del 2036.

**Paso 5 — Clave privada del servidor y petición de certificado (CSR)**
```
📌 $ openssl genrsa -out private/srv.key 2048
📌 $ openssl req -new -key private/srv.key -out srv.csr -subj "/C=ES/O=Torrent \ S.L./CN=hmi.torrent.local"
```

**CREA LA CLAVE PRIVADA DEL SERVIDOR**

<img width="676" height="47" alt="image" src="https://github.com/user-attachments/assets/7f471ac1-14dc-4d82-a6e6-1663e3256907" />

**CREA LA SOLICITUD DE CERTIFICADO (CRS)**

<img width="785" height="55" alt="image" src="https://github.com/user-attachments/assets/1827abb8-2770-4285-b896-2dde95af19bd" />

<img width="556" height="130" alt="image" src="https://github.com/user-attachments/assets/a664af84-3c0b-4285-be15-9e8e5bd24f66" />

**Paso 6 — Indicar para qué nombres e IP es válido el certificado (SAN)**
```
📌 $ nano srv.ext
```
Escribir dentro del fichero:

subjectAltName = DNS:hmi.torrent.local, IP:IP_SERVIDOR
basicConstraints = CA:FALSE
keyUsage = digitalSignature, keyEncipherment
extendedKeyUsage = serverAuth


<img width="568" height="132" alt="image" src="https://github.com/user-attachments/assets/90ac90c4-009a-4d89-bfc1-b9f0716ec99a" />

**CA:FALSE:** Este certificado no puede usarse para firmar otros (solo la CA puede).
**keyUsage / extendedKeyUsage serverAuth:** limitan el uso de la clave a identificar a un servidor web.

**Paso 7 — La CA firma el certificado del servidor**
Este es el momento clave: la CA comprueba la petición y la firma con su clave privada. A partir de aquí, cualquiera que confíe en la CA puede confiar en el servidor. La opción -CAcreateserial asigna un número de serie único al certificado.

```
📌 $ openssl x509 -req -in srv.csr  -CA certs/ca.crt -CAkey private/ca.key -CAcreateserial \
 -out certs/srv.crt -days 365 -sha256 -extfile srv.ext
```

<img width="582" height="137" alt="image" src="https://github.com/user-attachments/assets/fc8e7f2a-8ae0-40cb-84d6-20c61f4897ed" />




