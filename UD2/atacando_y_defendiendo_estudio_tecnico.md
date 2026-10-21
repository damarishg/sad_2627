**Paso 1.** Leed las seis fuentes con ojos de atacante y encontrad al menos 10 datos que os servirían para atacar a la empresa. Anotadlos en esta tabla:

| Dato encontrado | Fuente (1-6) | ¿Para qué le sirve al atacante? | Contramedida propuesta |
|-----------------|--------------|---------------------------------|------------------------|
| Compartir planos a través de la nube | 1 | Puede poner un malware en un fichero y un empleado lo ejecuta | Poner permisos para que los clientes solo puedan leer |
| Trabajo remoto sin VPN ( no la menciona | 1 | Si averigua la IP para conectarse en remoto puede acceder a los equipos de trabajo | Uso de VPN |
| Uso de Ubuntu 8.04 (obsoleto)| 2 | Puede ver por Internet parches y puertos abiertos de esa máquina y ganar acceso fácilmente | Usar un Ubuntu con soporte |
| Uso de Windows 7 | 2 | No le hace falta buscar mucho para encontrar vulnerabilidades y explotarlas | Uso de Windows 11 |
| Red Wifi y ni por cable | 2 | Si averigua la red (IP) puede conectarse | Usar red cableada | 
| Poner datos sensibles en el registro del dominio | 3 | Al saber información sensible lo puede usar a su favor (ingeniería social) | No poner información sensible en el registro del dominio |
| Registro TXT con v=spf1 +all | 4 | Autoriza a cualquier IP enviar correos en nombre del dominio. Le permite hacer phishing | Cambiar la regla a -all |
| Ausencia de registros DKIM y DMARC | 4 | Los correos ni llevan firma digital ni política de validación. Facilita la suplantación de identidad | Implementar y configurar los registros DKIM y DMARC |
| Post-It | 5 | Al verse la información si accede al equipo puede entrar en él sin ningún problema | No tener a la vista contraseñas, y menos publicarlas en Internet |
| Teletrabajo | 5 | El atacante sabe que está en la cafetería y que está conectada al Wifi de la misma. Puede interceptar sin problemas la red pública (Man-in-the-Middle) | No usar redes públicas y en el caso de tener que hacerlo usar un VPN |
| Ausencia en el trabajo | 6 | El atacante ve que no va a estar en la oficina y puede aprovechar para ir físicamente y robar o destruir información | No poner si alguien está ausente o no |
| Sin acceso al correo | 6 | En el caso de que el atacante tenga acceso al correo o haga un SMPT spoofing el trabajador no se entera de nada hasta que el ataque se ha realizado | No poner nunca que no va a tener acceso a X cosa |
| Información personal | 6 | El atacante sabe el nº de teléfono, lo puede usar para suplantación de identidad de clientes o smishing | No poner nunca información personal por correo |

**Paso 2.** Con lo que habéis encontrado, escribid en 5 líneas como máximo el plan de ataque que montaríais contra Estudio Torrent.

