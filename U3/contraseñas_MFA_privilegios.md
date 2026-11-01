#  PARTE 1 Contraseñas robustas con PAM
## 1.1 ¿Por qué PAM y no tocar /etc/shadow directamente?
PAM (Pluggable Authentication Modules) es la capa que usa Linux para gestionar la autenticación de cualquier
programa: login, su, passwd, sshd... Configurar la política de contraseñas en PAM significa que se aplica una sola
vez y afecta a todos los programas que autentican usuarios, en vez de tener que vigilar cada aplicación por
separado.

## 1.2 Instalar y activar pwquality

## 1.3 Práctica guiada — aplicar la política


## 1.4 Caducidad y aviso con chage


#  PARTE 2 MFA en SSH con Google Authenticator


## 2.1 El concepto: TOTP


## 2.2 Instalar el módulo PAM


## 2.3 Generar el secreto (lo hace cada usuario, con su propia cuenta)


## 2.4 Conectar Google Authenticator con sshd

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


