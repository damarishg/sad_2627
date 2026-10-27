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

planos/ -> Propietario: victor (es el que más permisos tiene sobre la carpeta)
```
📌 $ chown -R victor:ingenieria /srv/torrentsl/planos
```


### Tarea 3 — Configurar las ACL con setfacl




