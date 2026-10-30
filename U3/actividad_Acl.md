# Configuración de ACL en Linux

## Tabla de permisos
| Grupo | Recurso | Usuario | rwx |
|-------|---------|----------|-----|
| operarios | hmi_planta | TODOS | --x |
| operarios | planos/autocalve.dwg | alberto | r-- |
| operarios | planos/caldera.dwg | oscar | r-- |
| ingeniería | hmi_planta | nestor | r-x|
| ingeniería | hmi_planta | lucia | rwx|
| ingeniería | planos/autocalve.dwg | lucia | rw- |
| ingeniería | planos | victor | rwx |
| supervisores | planos | TODOS | r-x |


### Tarea 1 — Preparar el entorno

**1. Comprueba que el soporte de ACL está disponible en tu sistema:**
```
📌 $ dpkg -s acl
```

**2. Crea los tres grupos y los nueve usuarios del enunciado, asignando a cada usuario su grupo
correspondiente.**
Usuario:
```
📌 $ useradd – m usuario
```

Grupo:
```
📌 $ groupadd grupo
```

Añadir usuario a grupo:
```
📌 $ usermod -a -G grupo usuario
```

**3. Crea la estructura de ficheros: un fichero (o script) ejecutable llamado hmi_planta, y una carpeta planos/ con los ficheros autoclave.dwg, caldera.dwg y destilacion.dwg.**

**1.**Creo una carpeta para que esté todo dentro de ella:
```
📌 $ mkdir -p /srv/torrentsl
```
**2.**Crear el script dentro de torrentsl (dentro poner un echo para comprobar que los permisos están bien):
```
📌 $ nano /srv/torrentsl/hmi_planta.sh
```
**3.** Crear la carpeta planos con los archivos .dwg:
```
📌 $ nano /srv/torrentsl/planos/*.dwg
```

<img width="820" height="167" alt="image" src="https://github.com/user-attachments/assets/fb98af40-ab54-478b-8409-442a953426d8" />


### Tarea 2 — Permisos base (propietario / grupo / resto)
**4. Decide qué usuario y qué grupo deben ser propietarios de hmi_planta y de planos/ (por ejemplo, un usuario de sistemas o de ingeniería).**

hmi_planta -> Propietario: lucia (es la que más permisos tiene sobre el script)
```
📌 $ chown lucia:ingenieria /srv/torrentsl/hmi_planta.sh
```

<img width="687" height="100" alt="image" src="https://github.com/user-attachments/assets/99a40203-3c27-48a5-ab67-a42fddd5a6aa" />


planos/ -> Propietario: victor (es el que más permisos tiene sobre la carpeta)
```
📌 $ chown -R victor:ingenieria /srv/torrentsl/planos
```
<img width="697" height="122" alt="image" src="https://github.com/user-attachments/assets/56ce0368-df82-449b-a4c4-f601784d206b" />

**5.  Aplica con chmod unos permisos de partida restrictivos (por ejemplo 750 o 700), de forma que el resto del acceso tenga que concederse explícitamente mediante ACL.**

Permisos para hmi_planta.sh, he decidido poner 700 para que nadie más que ella tenga permisos en el sh (de momento):
```
📌 $ chmod 700 /srv/torrentsl/hmi_planta.sh
```

<img width="681" height="120" alt="image" src="https://github.com/user-attachments/assets/de37d520-5e30-488e-9e6e-69687f26c608" />


Permisos para la carpeta planos, he decidido poner 750 para que al menos los de su grupo puedan acceder a la carpeta (de momento):
```
📌 $ chmod 750 /srv/torrentsl/planos/
```

<img width="682" height="137" alt="image" src="https://github.com/user-attachments/assets/aca0638c-2ae8-45e3-afcb-b3601eef8d29" />


### Tarea 3 — Configurar las ACL con setfacl
**6- Aplica las ACL necesarias sobre hmi_planta**
```
📌 setfacl -m g:operarios:r-x /srv/torrentsl/hmi_planta.sh
```

<img width="833" height="45" alt="image" src="https://github.com/user-attachments/assets/8eec6bde-5115-4b69-8f28-dde5767136aa" />

Nestor:
```
📌 setfacl -m u:nestor:r-x /srv/torrentsl/hmi_planta.sh
```

<img width="821" height="75" alt="image" src="https://github.com/user-attachments/assets/82d59c59-2923-46d0-a078-a81b3f15766d" />


***7- Aplica las ACL necesarias sobre la carpeta planos/ y, si procede, sobre los ficheros que ya contiene**
Permisos para el grupo Supervisores:
```
📌 setfacl -R -m g:supervisores:r-x /srv/torrentsl/planos/
```
<img width="825" height="50" alt="image" src="https://github.com/user-attachments/assets/69f4d383-7e20-4641-a925-1e83068ed2b6" />

Permisos para el resto de usuarios:

Lucia:
```
📌 setfacl -m u:lucia:rw- /srv/torrentsl/planos/autoclave.dwg
```

<img width="840" height="62" alt="image" src="https://github.com/user-attachments/assets/294f5fcc-457e-40e0-8348-fe54e99fa8dd" />

Alberto: 
```
📌 setfacl -m u:alberto:r-- /srv/torrentsl/planos/autoclave.dwg
```

<img width="831" height="60" alt="image" src="https://github.com/user-attachments/assets/16cc9c78-3836-41f4-b345-56ae1fc08d41" />

Oscar:
```
📌 setfacl -m u:oscar:r-- /srv/torrentsl/planos/caldera.dwg
```
<img width="833" height="77" alt="image" src="https://github.com/user-attachments/assets/2e6e4823-51b7-408e-b3ad-4bf9641a768d" />

**8- Valora si alguno de los permisos del diagrama debería aplicarse también como ACL por defecto (setfacl -d), para que los ficheros nuevos que se creen en planos/ hereden automáticamente esos permisos.**

Creo que los permisos sobre la carpeta planos si que tendría que tener los permisos para que los hereden automáticamente.

```
📌 setfacl -R -d -m g:supervisores:r-x /srv/torrentsl/planos/
```

<img width="836" height="75" alt="image" src="https://github.com/user-attachments/assets/cf54dd9d-5f24-4088-8536-c730644f83de" />


### Tarea 4 Verificación
**9- Comprueba el resultado con getfacl sobre cada recurso:**

 hmi_planta:
 ```
📌 getafacl /srv/torrentsl/hmi_planta.sh
```

<img width="660" height="250" alt="image" src="https://github.com/user-attachments/assets/da611d3d-a56d-4d85-b1a0-5c12ceb32bb4" />


planos:
```
📌 getafacl /srv/torrentsl/planos/
```

<img width="647" height="352" alt="image" src="https://github.com/user-attachments/assets/01c6a48d-eabe-4e7c-87f3-e833e1f8ea53" />

**10- .Accede como cada uno de los nueve usuarios (su -l usuario) y comprueba que puede hacer exactamente lo que el diagrama indica, y nada más.**

**hmi_planta:**
1- operarios (ALBERTO):
 ```
📌 /srv/torrentsl/hmi_planta.sh
```

<img width="540" height="52" alt="image" src="https://github.com/user-attachments/assets/a6826794-24fe-4596-8a57-70f5b78e8a34" />

2- Nestor:
 ```
📌 /srv/torrentsl/hmi_planta.sh
```
<img width="541" height="50" alt="image" src="https://github.com/user-attachments/assets/f10aca8c-ccf0-4b5b-981d-34b4c3f0ec7b" />

3- Lucia:
 ```
📌 /srv/torrentsl/hmi_planta.sh
```

<img width="507" height="57" alt="image" src="https://github.com/user-attachments/assets/ac6fe46a-c73e-4663-8f41-46c0fe0d11c1" />

**planos:**
1- Victor:
 ```
📌 ls -l /srv/torrentsl/planos/
```

<img width="752" height="132" alt="image" src="https://github.com/user-attachments/assets/704025a6-db0c-43c0-88bb-dc4155176923" />

2- Supervisores (JOHN):
 ```
📌 ls -l /srv/torrentsl/planos/
```

<img width="692" height="128" alt="image" src="https://github.com/user-attachments/assets/7ad8e12a-60e0-49f5-a2f8-cd5fcf3d212c" />

3- Oscar:
 ```
📌 ls -l /srv/torrentsl/planos/
```
<img width="753" height="87" alt="image" src="https://github.com/user-attachments/assets/dc1d8af2-f817-434d-9788-0251588691cc" />
