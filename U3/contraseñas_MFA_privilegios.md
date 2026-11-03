#  PARTE 1 Contraseñas robustas con PAM
## 1.1 ¿Por qué PAM y no tocar /etc/shadow directamente?
PAM (Pluggable Authentication Modules) es la capa que usa Linux para gestionar la autenticación de cualquier
programa: login, su, passwd, sshd... Configurar la política de contraseñas en PAM significa que se aplica una sola
vez y afecta a todos los programas que autentican usuarios, en vez de tener que vigilar cada aplicación por
separado.

## 1.2 Instalar y activar pwquality
```
📌 $ sudo apt update

📌 $ sudo apt install libpam-pwquality
```

<img width="583" height="52" alt="image" src="https://github.com/user-attachments/assets/cdb9eab8-c72c-4e0e-a9fd-275f70543eec" />


## 1.3 Práctica guiada — aplicar la política

### 1- Edita /etc/security/pwquality.conf con sudo y fija los valores de la tabla anterior (descomenta la línea o añádela si no existe).

Editar el archivo con:
```
📌 $ sudo nao /etc/security/pwquality.conf
```

<img width="601" height="60" alt="image" src="https://github.com/user-attachments/assets/98d13ae4-7989-4ee1-9be3-b3cd53fcf542" />

**minlen:**

<img width="247" height="75" alt="image" src="https://github.com/user-attachments/assets/50b163ca-4a6e-40e1-86ef-cac84168e1ec" />

**dcredit:**

<img width="187" height="67" alt="image" src="https://github.com/user-attachments/assets/f6c625da-cfcb-479d-8dd7-1bde0d46e0ae" />

**ucredir:**

<img width="152" height="63" alt="image" src="https://github.com/user-attachments/assets/b2b77b9a-e38e-48f8-a49f-d1c0867139e5" />

**lcredit:**

<img width="163" height="67" alt="image" src="https://github.com/user-attachments/assets/afa62192-1ded-4ac0-a8de-cd62e47f8e28" />

**ocredit:**

<img width="155" height="62" alt="image" src="https://github.com/user-attachments/assets/b0c46cd6-bb14-4da0-bf1f-89faae2f561b" />

**minclass:**

<img width="173" height="72" alt="image" src="https://github.com/user-attachments/assets/bf521288-bd35-42a1-85f2-2e61e3113c9c" />

**dictcheck:**

<img width="180" height="76" alt="image" src="https://github.com/user-attachments/assets/a6a8193d-65c6-4760-b767-d2f65f092062" />

**maxrepeat:**

<img width="210" height="87" alt="image" src="https://github.com/user-attachments/assets/80dd42f6-cead-4221-b7ab-4988d61b4274" />

**enforcefor_root:**

<img width="207" height="67" alt="image" src="https://github.com/user-attachments/assets/bac106a1-5541-4e23-a2b4-f8bd731ab649" />

### 2- Comprueba que la línea pam_pwquality.so está presente en /etc/pam.d/common-password.
```
📌 $ grep -n "pam_pwquality.so" /etc/pam.d/common-password
```
<img width="817" height="72" alt="image" src="https://github.com/user-attachments/assets/449a57ef-d0cd-4f30-af64-ec5b318b9814" />

<img width="828" height="71" alt="image" src="https://github.com/user-attachments/assets/d3a8a6e1-9b2d-4e62-9090-fdbcfd9ab5c9" />

### 3- Como el usuario hugo (grupo operarios, creado ayer en la práctica de ACL), intenta cambiarle la contraseña por algo débil y observa el rechazo:
```
📌 $ sudo passwd hugo
```
<img width="723" height="102" alt="image" src="https://github.com/user-attachments/assets/90a453cc-b7cc-41a2-8588-8af167f9d205" />

### 4- Repite con una contraseña que cumpla la política y confirma que esta vez se acepta
```
📌 $ sudo passwd hugo 
```
Contraseña: DamaDama0123.

<img width="457" height="87" alt="image" src="https://github.com/user-attachments/assets/adf9ece1-c819-47ab-82b9-4c0b8eb48d56" />


### 5- Como root, intenta ponerle a root mismo una contraseña débil: sin enforce_for_root se aceptaría sin más. Actívala y repite la prueba.
```
📌 $ sudo passwd root 
```
<img width="710" height="110" alt="image" src="https://github.com/user-attachments/assets/0923b106-c233-4fa1-814a-1b051beeab94" />


## 1.4 Caducidad y aviso con chage
pwquality decide si una contraseña es lo bastante fuerte al crearla; chage decide durante cuánto tiempo sigue siendo válida.

<img width="917" height="227" alt="image" src="https://github.com/user-attachments/assets/0707c8c3-9380-4b24-98fd-71667bf33fbf" />


<img width="750" height="80" alt="image" src="https://github.com/user-attachments/assets/6cacb3b4-633f-4606-b08d-98610c7faa84" />

<img width="597" height="85" alt="image" src="https://github.com/user-attachments/assets/587662eb-b9d4-411f-bbe2-1689b2598f84" />

<img width="757" height="215" alt="image" src="https://github.com/user-attachments/assets/cce58345-17b4-4cfa-8089-c91042fba4da" />

#  PARTE 2 MFA en SSH con Google Authenticator
## 2.1 El concepto: TOTP
TOTP (Time-based One-Time Password) genera un código de 6 dígitos que cambia cada 30 segundos, a partir de un secreto compartido entre el servidor y tu app (Google Authenticator, Authy, FreeOTP...) y la hora actual. 
Aunque alguien robe tu contraseña, sin ese secreto no puede generar el código — por eso es un segundo factor («algo que tienes»), no solo «algo que sabes».

## 2.2 Instalar el módulo PAM
```
📌 $ sudo apt install libpam-google-authenticator
```

<img width="697" height="45" alt="image" src="https://github.com/user-attachments/assets/6336b3cf-91a3-4823-8823-ade7b03d22ff" />


## 2.3 Generar el secreto (lo hace cada usuario, con su propia cuenta)
```
📌 $ su - hugo
```

<img width="317" height="80" alt="image" src="https://github.com/user-attachments/assets/294143a2-1c77-4bc1-980f-6a4053af32a5" />

**⚠️ Instalar Google Authenticator en tu móvil**

```
📌 $ google-authenticator
```

<img width="456" height="66" alt="image" src="https://github.com/user-attachments/assets/a4d2f6fa-b1ff-4979-90bd-6f1680b92be9" />


<img width="770" height="268" alt="image" src="https://github.com/user-attachments/assets/6d95a2e9-cb1a-447a-a237-3ada45dbd09c" />

## 2.4 Conectar Google Authenticator con sshd
Añade esta línea al final de /etc/pam.d/sshd (después de @include common-auth, para que primero se compruebe la contraseña y después el código):

```
📌 auth required pam_google_authenticator.so
```

**⚠️ Tienes que tener instalado SSH para poder hacerlo**
```
📌 sudo apt install openssh-server
```



## 2.5 Verificación


## 2.6 ¿A quién se lo exigimos?


#  PARTE 3 Privilegios justos con sudo

## 3.1 Por qué no usar la cuenta root a diario


## 3.2 Sintaxis básica de sudoers


## 3.3 Práctica guiada


## 3.4 Verificación


# Preguntas de reflexión

### 1- Una contraseña de 20 caracteres aleatorios es, en teoría, casi imposible de reventar por fuerza bruta. ¿Qué riesgo concreto cubre el MFA que una contraseña así, por fuerte que sea, no cubre?


### 2- Buscad brevemente la postura actual del NIST sobre la caducidad obligatoria de contraseñas. ¿Por qué forzar el cambio cada pocos días puede llegar a ser contraproducente para la seguridad real?


### 3- Aunque sudo esté perfectamente configurado, seguís necesitando las ACL de la sesión anterior. Explicad con un ejemplo por qué una capa no sustituye a la otra

### 4- Un alumno pierde el móvil con Google Authenticator y no guardó los scratch codes. ¿Qué tendría que hacer el administrador del sistema para devolverle el acceso sin comprometer la seguridad del resto de cuentas?


