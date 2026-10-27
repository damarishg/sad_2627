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
| supervisores | planos | TODOS | rwx |


### Tarea 1 — Preparar el entorno

1. Comprueba que el soporte de ACL está disponible en tu sistema:
```
📌 $ dpkg -s acl
```

### Tarea 2 — Permisos base (propietario / grupo / resto)



### Tarea 3 — Configurar las ACL con setfacl




