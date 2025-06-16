
---------------

>CrackMapExec es una herramienta bastante poderosa de la suite de Impacket. Es comúnmente usada para atacar servicios SMB y redes de Directorio Activo, permite desde enumerar recursos y probar credenciales hasta ataques de post explotación, ejecuciones de comandos y Pass The Hash.

>El ejemplo mas sencillito puede ser el ataque a un servicio SMB, con un usuario y una lista de contraseñas:

![\1](Attachments/Pasted%20image%2020250604165420.png)
1. _crackmapexec Es la herramienta que usaremos para el ataque contra SMB_
2. _con "smb" especificamos el servicio a atacar (En el puerto 445 por defecto)_
3.  proporcionamos la IP con el servicio a atacar
4. con la flag "-u satriani7" indicamos el usuario
5. con "-p wordlist" proporcionamos la lista de posibles contraseñas que deseamos probar
