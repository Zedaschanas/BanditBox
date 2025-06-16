

-------------------

>Hoy tenemos la maquina FirstHacking, bastante, BASTANTE simple, pero muy interesante
   en su esencia…
>Empezamos por levantar la maquina:

![FirstHacking](Attachments/FirstHacking.png)

>Ahora, hacemos un escaneo de puertos con nmap:

![FirstHacking](Attachments/FirstHacking%201.png)
1. _-- min-rate 5000 (quiero tramitar mínimo 5000 paquetes por segundo) esto para que el escaneo vaya con bastante agilidad._
2. _-n (no deseo que nmap haga una resolución DNS automática)
3. _-p- (quiero escanear los 65535 puertos del sistema, no los 1000 más comunes, como normalmente hace nmap)._
4. _-Pn (omite el descubrimiento de Host)_
5. _-sV (realiza un reconocimiento de versiones de servicios)_

>Particularmente, notamos que solamente hay un servicio FTP corriendo en el puerto 21, con una versión un tanto desactualizada del servicio Vsftpd
>Searchsploit nos indica que la versión es vulnerable:

![FirstHacking](Attachments/FirstHacking%202.png)
_Searchsploit es una herramienta de terminal que filtra y busca entre todos los exploits que se encuentran en [*ExploitDB*](https://www.exploit-db.com/). _En el paquete preinstalado en kali "exploitdb" existe un archivo files_exploits.csv en donde se encuentran los metadatos de TODOS los exploits.
![\1](Attachments/Pasted%20image%2020250504142751.png)
La herramienta "Searchsploit" filtra por todos ellos buscando coincidencias por sistema operativo, descripción o nombre del servicio, hasta encontrar una coincidencia para una vulnerabilidad que estemos buscando._

>Así que, vamos a traernos ese exploit al directorio actual de trabajo:

![FirstHacking](Attachments/FirstHacking%203.png)
_Con el parámetro -m (mirror) creamos una copia en el directorio de trabajo actual del exploit que deseamos. Searchsploit copia este archivo desde una ruta en particular del sistema en la que se almacenan los exploits del paquete "ExploitDB" _
![\1](Attachments/Pasted%20image%2020250504143519.png)

>Con un poco de ayuda, logramos interpretar el código dentro del exploit, bastante sencillo en realidad.

>Luego de investigar un poco, en síntesis, Esta versión del servicio (vsftpd 2.3.4.) fue crackeada, y el servicio infectado se esparció con rapidez, en él, un Cracker inserto al código fuente un backdoor, así cuando un usuario se intenta autenticar con cualquier nombre y una carita feliz, se abrirá el puerto 6200 de la máquina, que otorga una /bin/sh a quien se conecte.

_Si quieres entender un poco más de este hackeo aquí te dejo un poco de lo mejor que encontré (_[_https://github.com/puckiestyle/exploit-CVE-2011-2523_](https://github.com/puckiestyle/exploit-CVE-2011-2523)

_Este repositorio que contiene la versión Infectada del código fuente de Vsftpd.), (_[_https://seclists.org/oss-sec/2011/q3/119_](https://seclists.org/oss-sec/2011/q3/119)


>Bien, vamos a explotarlo manualmente, para que sea más interesante, primero, nos intentamos autenticar, puede ser con Telnet, ftp o nc:

![FirstHacking](Attachments/FirstHacking%204.png)

>El usuario que insertamos puede ser cualquiera, pero, para activar el backdoor debe tener al final una carita feliz :)

>Por otro lado la contraseña puede ser cualquiera.

>Si analizamos el puerto 6200 nuevamente, nos encontramos con que ahora está abierto

![FirstHacking](Attachments/FirstHacking%205.png)

>Ya solo queda conectarnos con nc por ese mismo puerto, como el servicio Vsftpd corre con privilegios de root, la Shell que obtenemos tiene privilegios máximos

![FirstHacking](Attachments/FirstHacking%206.png)

>También lo puedes lograr solo con el one liner:

```bash
printf “USER bandit:)\r\nPASS pass\r\n” | nc 172.17.0.2 && sleep 1 && nc 172.17.0.2 6200
```
_Funciona 1 de cada 3 veces :( 

![FirstHacking](Attachments/FirstHacking%207.png)
O.O   //El comando "rm -rf /*" ejecutado como administrador borra todos los archivos del sistema, es un pequeño guiño al ganar acceso como root a una máquina, NO LO EJECUTES//   O.O