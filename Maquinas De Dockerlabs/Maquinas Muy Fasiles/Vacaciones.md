

---
>Hoy, una maquina muy muy facilita, vamos allá...

![\1](Attachments/Vacaciones.png)

>Primero desplegamos la maquina

![Vacaciones](Attachments/Vacaciones%201.png)
1. sudo: Ejecuta como superusuario El siguiente comando
2. auto_deploy: Herramienta de DockerLabs para levantar máquinas.
3. vacaciones.tar: El "paquete" de la máquina vulnerable.
---

>Ya con la maquina desplegada, hacemos un escaneo a profundidad con nmap:

![Vacaciones](Attachments/Vacaciones%202.png)
1. -p-: Escanea todos los puertos (65535 puertos en total).
2. -sV: Detecta versiones de servicios escaneados
3. --min-rate 5000: no envía paquetes mas lento que 5000 por segundo, es decir va volando

>Nos encontramos con el puerto 22 con OpenSSH 7.6 y el puerto 80 con Apache 2.4.29.

![Vacaciones](Attachments/Vacaciones%203.png)

>Al revisar la web, la misma esta vacía, así que procedemos revisar el código fuente, allí si que hay cosas interesantes:

![Vacaciones](Attachments/Vacaciones%204.png)

>Con este mensaje super escondido, ya podremos intentar romper el servicio ssh con fuerza bruta.

![Vacaciones](Attachments/Vacaciones%205.png)
1. -l camilo: Atacamos al usuario "Camilo" (encontrado en el código HTML).
2. -P rockyou.txt: Usamos el diccionario rockyou.txt (El de siempre).
3. -t 64: 64 hilos paralelos (Máximo de hilos que permite la herramienta).

Contraseña encontrada: password1, Así que ingresamos al servicio SSH

![Vacaciones](Attachments/Vacaciones%206.png)

>Recordando un poco, el mensaje encontrado en la web era de juan para camilo, hablando sobre un correo importante, así que, naturalmente, revisamos el lugar donde se almacenan los correos en Linux, /var/mail

![Vacaciones](Attachments/Vacaciones%207.png)

>Fácilmente encontramos una contraseña en texto claro, con las que podemos cambiar de usuario

![Vacaciones](Attachments/Vacaciones%208.png)

>Con un “sudo -l” Listamos los privilegios que tiene juan en todo el sistema, y…

![Vacaciones](Attachments/Vacaciones%209.png)

>Juan puede ejecutar /usr/bin/ruby como root sin contraseña.


![Vacaciones](Attachments/Vacaciones%2010.png)
- GTFOBins nos ilumina

>Vemos una manera muy simple de escalar privilegios, así que:

![Vacaciones](Attachments/Vacaciones%2011.png)

---
O.O   //El comando "rm -rf /*" ejecutado como administrador borra todos los archivos del sistema, es un pequeño guiño al ganar acceso como root a una máquina, NO LO EJECUTES//   O.O
