
---------------

>Rpcclient es una herramienta que nos permite interactuar con servicios RPC (Remote Procedure Call) como Samba, se conecta al servidor con un usuario especifico y nos entrega un Prompt (rpcclient $>) en donde podemos ejecutar diferentes comandos y extraer información sensible dependiendo de la seguridad del servidor al que hacemos la conexión. Por lo general, al enumerar Samba, se listan usuarios (enumdomusers),  información sobre ellos (queryuser rid), grupos (enumdomgroups) o información del servidor (srvinfo)_

>Un pequeño ejemplo, seria la enumeración e un servicio Samba con una NULL SESSION:

![\1](/Attachments/Pasted%20image%2020250604163130.png)
_En este caso con la flag -U "" nos conectamos como usuario Anonymous, con la flag -N "no pass" indicamos que no proporcionamos credenciales. Para por ultimo listar usuarios del servidor con "Enumdomusers"

>Rpcclient es solamente el cliente de terminal del protocolo RPC que se utiliza en diferentes servicios y sistemas. Aunque el ejemplo anterior es limitado, y podríamos haber obtenido el mismo resultado con una herramienta automática, en realidad Rpcclient es una herramienta que vale la pena saber usar en entornos de directorio activo o NFS.