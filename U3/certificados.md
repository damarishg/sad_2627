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


¿Por qué el certificado del servidor dura 365 días y el de la CA 10 años?

Porque el CA lo he creado para que tenga una vigencia de 3650 días (10 años) ya que es el certificado principal que usaré para firmar otros certificados, mientras que el certificado del servidor le he puesto 365 días (1 año) y es aconsejable renovarlo con más frecuencia por seguridad.

**Paso 8 — Verificar la cadena de confianza**
```
📌 $ openssl verify -CAfile certs/ca.crt certs/srv.crt
```

<img width="775" height="55" alt="image" src="https://github.com/user-attachments/assets/11a04071-0d86-4aac-9114-517c5b2ac596" />

```
📌 $ openssl x509 -in certs/srv.crt -noout -subject -issuer -ext subjectAltName
```

<img width="825" height="147" alt="image" src="https://github.com/user-attachments/assets/51c1b783-22e0-4fea-8afc-67259a020615" />

**Paso 9 — Instalar el certificado en Apache**
Copiamos el certificado y la clave a los directorios estándar del sistema y activamos el módulo SSL de Apache. La clave solo debe poder leerla root (Apache la lee al arrancar, antes de cambiar de  usuario).

* **Paso 1:** Copiar el  certificado a su directorio
 ```
📌 $ sudo cp ~/ca/certs/srv.crt /etc/ssl/certs/torrent-srv.crt
 ```

<img width="831" height="55" alt="image" src="https://github.com/user-attachments/assets/fe095155-3f33-4a1a-be93-68f11f1d2b6e" />

* **Paso 2:** Copiar la clave privada
```
📌  sudo cp ~/ca/private/srv.key /etc/ssl/private/torrent-srv.key
```

<img width="875" height="41" alt="image" src="https://github.com/user-attachments/assets/59e23f27-537d-4fb6-a302-e6e66064a277" />

* **Paso 3:** Proteger la clave privada
```
📌 $ sudo chmod 600 /etc/ssl/private/torrent-srv.key
```

<img width="737" height="51" alt="image" src="https://github.com/user-attachments/assets/cc16e13e-33a6-4ab3-9679-5fe182116c6c" />

* **Paso 4:** Activar el módulo SSL de Apache
```
📌 $ sudo a2enmod ssl
```

<img width="411" height="45" alt="image" src="https://github.com/user-attachments/assets/96810343-0b57-4386-91aa-04c51a723d56" />

<img width="565" height="40" alt="image" src="https://github.com/user-attachments/assets/c4662666-47d5-4e6f-bec8-4a9c3c4eeac9" />

* **Paso 5:** Modificar la configuración SSL de Apache
```
📌 $ sudo nano /etc/apache2/sites-available/default-ssl.conf
```
Antes de configurar:

<img width="653" height="80" alt="image" src="https://github.com/user-attachments/assets/b4e9cd3e-ca31-48e3-93c2-5d04e8f89b51" />

Después de configurar:

<img width="591" height="77" alt="image" src="https://github.com/user-attachments/assets/b3670916-4d0d-40c1-a414-762861cad28f" />

* **Paso 6:** Activar el sitio HTTPS
```
📌 $ sudo a2ensite default-ssl
```

<img width="505" height="42" alt="image" src="https://github.com/user-attachments/assets/431eaa37-48fd-49d3-846f-530e28378f48" />

* **Paso 7:** Comprobar la configuración
```
📌 $ sudo apache2ctl configtest
```

<img width="587" height="121" alt="image" src="https://github.com/user-attachments/assets/ae31b9ff-cd58-4993-b72a-b4c139307783" />

* **Paso 8:** Reiniciar Apache
```
📌 $ sudo systemctl restart apache2
```

<img width="581" height="42" alt="image" src="https://github.com/user-attachments/assets/0c644276-0dfd-4e74-a506-00615df65505" />

* **Paso 9:** Comprobar que Apache escucha por el puerto 443
```
📌 $ sudo ss -tlnp | grep ':443'
```

<img width="880" height="93" alt="image" src="https://github.com/user-attachments/assets/3af307ca-c68c-4075-b41f-f3f58de2f2f0" />

**Paso 10 — Probar HTTPS desde la propia VM, antes de confiar en la CA**

* **1:** Probar HTTPS sin confiar todavía en la CA
```
📌 $ curl -I https://192.168.56.2(IP_SERVIDOR)
```

<img width="811" height="182" alt="image" src="https://github.com/user-attachments/assets/6e0c21b9-1081-4f7d-82a3-9a94d9000e38" />

* **2:** Probar confiando en la CA
```
📌 $ curl -I --cacert ~/ca/certs/ca.crt https://192.168.56.2(IP_SERVIDOR)
```

<img width="821" height="55" alt="image" src="https://github.com/user-attachments/assets/78e87d99-51ab-474b-a181-a30c606ca905" />

Explica con tus palabras por qué el primero falla y el segundo funciona.

En el primer intento falla porque Ubuntu no confía en la CA ya que su certificado es autofirmado y todavía no está instalada entre las autoridades de confianza. En el segundo funciona porque le he indicado expresamente a *curl* que confíe en la CA mediante la opción *--cacert*.

**Paso 11 — El navegador: advertencia, huella e importación de la CA**
* **1:** Advertencia de seguridad inicial en Firefox por certificado no confiable

<img width="892" height="701" alt="image" src="https://github.com/user-attachments/assets/fc205e2f-d726-4956-8062-74032d0e6405" />

<img width="363" height="80" alt="image" src="https://github.com/user-attachments/assets/610d333f-f26a-4707-afbf-1870d631eb10" />

* **2:** Publicar el certificado de la CA
```
📌 $ sudo cp ~/ca/certs/ca.crt /var/www/html/torrent-ca.crt
```

<img width="808" height="43" alt="image" src="https://github.com/user-attachments/assets/809b80fc-6162-445b-9195-7e6928f17b27" />


* **3:** Calcular la huella digital de la CA
```
📌 $ openssl x509 -in ~/ca/certs/ca.crt -noout -fingerprint -sha256
```

<img width="891" height="70" alt="image" src="https://github.com/user-attachments/assets/7a513bd7-6850-4339-ac9a-af2fc5f4f223" />

* **4:** Abrir el certificado en Firefox

Certificado descargado:

<img width="447" height="167" alt="image" src="https://github.com/user-attachments/assets/5bda1a5b-e82b-48d7-bbd2-5fa56565fd80" />

Para instalarlo hay que ir a Ajustes - Privacidad y Seguridad- Certificados- Ver Certificados - Autoridades - Importar - Confiar en esta CA para identificar sitios web

<img width="860" height="266" alt="image" src="https://github.com/user-attachments/assets/c193ffaf-d330-49e5-b562-d234c67635c9" />

<img width="685" height="223" alt="image" src="https://github.com/user-attachments/assets/44adce04-1a74-49c5-af30-2315e2e6e088" />

* **5:** Comprobación

<img width="641" height="275" alt="image" src="https://github.com/user-attachments/assets/c4368e90-695a-457f-8942-b095df0507cd" />

<img width="277" height="106" alt="image" src="https://github.com/user-attachments/assets/5d4be3ce-279a-404c-ad30-f5146e392e92" />

**Paso 12 — Mirar la conexión TLS por dentro**
```
📌 $ openssl s_client -connect 192.168.56.2:443 -CAfile ~/ca/certs/ca.crt </dev/null 2>/dev/null | grep -E 's:|i:|Verify return code|Cipher'
```

<img width="872" height="150" alt="image" src="https://github.com/user-attachments/assets/52e0398d-5f8d-4dc9-96f1-323141a27fec" />

* **Versión del TLS:** TLS 1.3
* **Cifrado negociado:** TLS_AES_256GCM_SHA384

**Prueba sin -CAfile**
```
📌 $ openssl s_client -connect 192.168.56.2:443 </dev/null 2>/dev/null | grep -E 's:|i:|Verify return code|Cipher'
```

<img width="893" height="235" alt="image" src="https://github.com/user-attachments/assets/d7e80fd7-1b24-4b7d-b21e-a3be29e446c2" />

Al quitar la opción -CAfile, OpenSSL devuelve el código 21, porque no dispone del certificado de la autoridad certificadora (AC) para verificar la cadena de confianza.

**Paso 13 — Guardar lo hecho para la Fase 2**
```
📌 $ tar czf ~/ca-torrent-backup.tar.gz -C ~ ca
```

<img width="712" height="47" alt="image" src="https://github.com/user-attachments/assets/a94a87d6-86ae-4798-a1b9-24fa8a2b264e" />

## 1.3 Preguntas de la Parte 1
**1. Explica con tus palabras la diferencia entre ca.key, ca.crt, srv.key y srv.crt: ¿cuáles son secretos y cuáles públicos?**

* **ca.key:** Es la clave privada de la autoridad certificadora (CA). Se usa para firmar certificados. Es secreta y NUNCA debe compartirse.
* **ca.crt:** Es el certificado público de la autoridad certificadora (CA). Sirve para que otros equipos puedan confiar en los certificados que firma. No es secreta.
* **srv.key:** Es la clave privada del servidor Apache. Se usa para demostrar su identidad durante las conexiones HTTPS.c Es secreta y debe protegerse.
* **srv.crt:** Es el certificado público del servidor. Identifica al servidor y contiene su clave pública. No es secreto.

**2. Alguien roba ca.key (sin contraseña). ¿Qué puede hacer con ella? ¿Qué tendría que hacer Torrent S.L. para recuperar la confianza?**

Puede usarla para firmar certificados falsos haciéndose pasar por la autoridad certificadora (CA) de Torrent S.L. Esto permitiría crear certificados fraudulentos que podrían ser aceptados como legítimos. 

Para recuperar la confianza, Torrent S.L tendría que revocar la CA comprometida, crear una nueva CA con una nueva clave privada, generar y distribuir un nuevo certificado público de la CA, volver a emitir los certificados de los servidores con la nueva CA e instalar el nuevo certificado de confianza en todos los equipos clientes.

**3. ¿Por qué descargar ca.crt por HTTP y no verificar la huella es un riesgo? ¿Quién podría aprovecharlo?**

Descargar *ca.crt* por HTTP sin verificar su huella es peligroso porque un atacante podría interceptar la descarga y sustituir el certificado original por uno falso.

Podría aprovecharlo un atacante que realice un ataca Man-in-the-Middle para conseguir que los usuarios confíen en una CA falsa.

**4. ¿Es una buena idea que la CA y el servidor web estén en la misma máquina? ¿Qué problema de seguridad crea?**

No es recomendable, porque si un atacante consigue acceder al servidor web, también podrían robar la clave privada de la CA (ca.key). Esto le permitiría crear certificados falsos y comprometer la confianza de toda la organización. Lo ideal es mantener la CA en una máquina separada.


#  PARTE 2 Un certificado real y gratuito con Let’s Encrypt
## 2.2 Paso a paso

**Paso 1 — Crear tu subdominio en DuckDNS**

<img width="767" height="143" alt="image" src="https://github.com/user-attachments/assets/0ba34a23-35a7-4af7-9292-d5b3ae7a0125" />

<img width="522" height="152" alt="image" src="https://github.com/user-attachments/assets/e991db99-6bd9-464b-b398-127806363009" />

<img width="617" height="70" alt="image" src="https://github.com/user-attachments/assets/ebb52f38-0ec9-4f05-aaf3-4f7b26ff3eed" />

**Paso 2 — Instalar acme.sh (leyendo antes lo que se va a ejecutar)**
**Paso 1:** Iniciar sesión como root
```
📌 $ sudo -i
```

<img width="361" height="42" alt="image" src="https://github.com/user-attachments/assets/ea57b448-8e73-4fc0-a39f-d70a54ba0e94" />

**Paso 2:** Descargar el script de instalación sin ejecutarlo
```
📌 # curl -o get-acme.sh https://get.acme.sh
```

<img width="818" height="116" alt="image" src="https://github.com/user-attachments/assets/6dbb4028-bd5e-4f11-ba1d-0b22566f409f" />

**Paso 3:** Abrir y leer el contenido de get-acme.sh
```
📌 less get-acme.sh
```

```
📌 sh get-acme.sh email=damaris@gmail.com
```

<img width="630" height="75" alt="image" src="https://github.com/user-attachments/assets/2f24774d-3053-41bc-b15e-351e92bd169a" />

<img width="531" height="75" alt="image" src="https://github.com/user-attachments/assets/3dfd1ebd-ab99-49fa-a319-0027572dfa84" />


El script get-acme.sh instala acme.sh, una herramienta que usa el protocolo ACME para comunicarse con autoridades certificadoras como Let's Encrypte. Esta herramienta permite solicitar, instalar y renovar automáticamente certificados SSl/TLS para habilitar conexiones HTTPS en el servidor Apache.

**Paso 3 — Elegir Let’s Encrypt como autoridad**
```
📌 # ~/.acme.sh/acme.sh --set-default-ca --server letsencrypt
```

<img width="897" height="77" alt="image" src="https://github.com/user-attachments/assets/c9bbfb77-80b8-446c-88b4-645a6795cec4" />


Este comando configura Let's Encrypt como autoridad certificadora (CA) predeterminada de acme.sh, para que sea la entidad encargada de emitir los certificados digitales SSL/TLS para HTTPS-

**Paso 4 — Pedir el certificado (desafío DNS-01)**

* **1:** Guardar el token de DuckDNS
```
📌 # export DuckDNS_Token="PEGA_AQUI_TU_TOKEN"
```

* **2:** Solicitar el certificado a Let's Encrypte usando DuckDNS
```
📌 # ~/.acme.sh/acme.sh --issue --dns dns_duckdns -d NOMBRE.duckdns.org
```

<img width="890" height="65" alt="image" src="https://github.com/user-attachments/assets/6f1dfc5e-3d22-436c-9763-e50d886d837c" />

<img width="706" height="142" alt="image" src="https://github.com/user-attachments/assets/727553fd-a638-4c21-9ca4-c5661b32aba7" />


* **DuckDNS:** Proporciona el dominio y permite modificar sus registros DNS.
* **Token:** Contraseña de acceso a la API de DuckDNC.
* **acme.sh:** Herramienta que automatiza la solicitud del certificado.
* **DNS-01:** Método que verifica la propiedad del dominio mediante un registro TXT.
* **Let's Encrypt:** Autoridad que emite el certificado SSL/TLS.

**Paso 5 — Instalar el certificado donde Apache pueda usarlo**
```
📌 # mkdir -p /etc/ssl/torrent-le
📌 # ~/.acme.sh/acme.sh --install-cert -d damarishg.duckdns.org --key-file /etc/ssl/torrent-le/privkey.pem --fullchain-file /etc/ssl/torrent-le/fullchain.pem --reloadcmd "systemctl reload apache2"
📌 # chmod 600 /etc/ssl/torrent-le/privkey.pem
```

<img width="900" height="117" alt="image" src="https://github.com/user-attachments/assets/af64270b-0312-4a5a-a641-216472a944f6" />

<img width="637" height="36" alt="image" src="https://github.com/user-attachments/assets/bbbb4a4c-a33e-40c2-a9e4-ec706b0e5e3e" />

* **privkey.pen:** Contiene la clave privada del servidor. Es secreta y NO debes compartirla.
* **fullchain.pem:** Contiene el certificado del dominio y los certificados intermedios necesarios para verificar la cadena de confianza.

**⚠️ .key indica que el archivo contiene una clave, mientras que .pem indica el formato en el que se guarda la información. Un archivo .pem también puede contener una clave privada.**

**Paso 6 — Cambiar Apache al certificado de Let’s Encrypt**
Hacemos una copia del fichero para poder volver al certificado de la CA de Torrent S.L. y cambiamos las dos rutas.
```
📌 # cp /etc/apache2/sites-available/default-ssl.conf /root/default-ssl.conf.ca-propia
📌 # nano /etc/apache2/sites-available/default-ssl.conf
```

<img width="905" height="78" alt="image" src="https://github.com/user-attachments/assets/ad21f49a-e5fd-4a79-9192-ec18424875bb" />

Hay que modificar las rutas SSLCertificateFile y SSLCertificateKeyFile del archivo anterior para que Apache use el nuevo certificado de Let's Encrypt en lugar del certificado de mi CA porpia. De esta manera, Apache usará el certificado público de Let's Encrypte y su clave privada correspondiente para establecer conexiones HTTPS seguras.

<img width="660" height="80" alt="image" src="https://github.com/user-attachments/assets/b1bd8b38-55d8-4a3b-aac5-3a5f9d285a53" />

Una vez cambiado el certificado y clave se comprueba que la sintaxis está bien y se reinicia Apache.

<img width="447" height="96" alt="image" src="https://github.com/user-attachments/assets/fdbfb1e9-d69e-429f-b2de-581c6701ec0f" />

<img width="526" height="37" alt="image" src="https://github.com/user-attachments/assets/bf1defec-80f9-4400-aeec-405b5c4a7f11" />

**Paso 7 — Comprobar el resultado**
Al haber cambiado la CA autofirmada por la de Let¡s Encrypte que es una autoridad certificadora pública (de confianza) reconocida por los sistemas y navegadores no es necesario poner -CAfile, ya que el equipo puede verificar el certificado usando las autoridades certificadores (CA) de confianza que tiene instaladas.

Comprobación con curl:

<img width="882" height="300" alt="image" src="https://github.com/user-attachments/assets/be18e91b-ab4a-4683-9ee9-9bd407ae31af" />

Comprobación desde el navegador:

<img width="635" height="257" alt="image" src="https://github.com/user-attachments/assets/5568bffd-ea94-46b4-93a8-414617547fd7" />

**Paso 8 — Mirar la cadena y la lista de CA de confianza**
Con s_client vemos qué envía el servidor. Fíjate en que ahora hay dos certificados en la cadena: el tuyo y el de una entidad intermedia. La raíz no se envía porque ya está en el sistema.
```
📌 # openssl s_client -connect damarishg.duckdns.org:443 -servername NOMBRE.duckdns.org </dev/null 2>/dev/null | grep -E ' s:| i:|Verify return code'
```

<img width="907" height="243" alt="image" src="https://github.com/user-attachments/assets/c344b111-0d4d-4eeb-9cea-d419b4b0d32b" />

```
📌 # ls /etc/ssl/certs | grep -i isrg
```

<img width="570" height="85" alt="image" src="https://github.com/user-attachments/assets/81fc8a33-7c56-459d-a969-de0f1ac260a9" />

La CA de Torrent S.L sí existe, ya que está creada, pero no aparece en /etc/ssl/certs porque no está instalada como autoridad certificadora (CA) de confianza en el sistema. Tener una CA creada no significa que el sistema confíe automáticamente en ella.

La lista de autoridades certificadores (CA) de confianza está gestionada por Ubuntu mediante el paquete *ca-certificates*. Los certificados llegan al PC a través de las actualizaciones del sistema o cuando un administrador los instala manualmente. Se almacenan en */etc/ssl/certs*.

* **default-ssl.conf:** Es un archivo de configuración de Apache. Le indica qué certificado y qué clave privada debe usar para proteger las conexiones HTTPS de la página web.
* **/etc/ssl/certs/:** Es una carpeta donde Ubuntu guarda los certificados de las autoridades certificadoras en las que confían para verificar certificados de otros servidores.

**Paso 9 — La renovación automática**
Los certificados de Let’s Encrypt duran 90 días. acme.sh programó en cron una tarea diaria que renueva el certificado cuando se acerca la fecha y recarga Apache con el comando que guardamos en el Paso 5.
```
📌 # crontab -l | grep acme
📌 # ~/.acme.sh/acme.sh --list
```

<img width="1225" height="147" alt="image" src="https://github.com/user-attachments/assets/d1d29f12-fdd6-4be3-8a01-5399eebbe89e" />

Cuándo caduca tu certificado y cuándo intentará renovarse.

El certificado de damarishg.duckdns.org caduca el 08/01/2027 y acme.sh tiene programada su renovación automática para esa misma fecha
