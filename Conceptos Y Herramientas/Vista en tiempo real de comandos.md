
------------

>Lo siguiente es una manera bastante sencilla y útil para observar el comportamiento de un comando ejecutado en tiempo real.
>En el ejemplo, somos el usuario "SAMARA", y vemos al listar procesos, que root ejecuto un archivo "echo.sh". Para verificar si fue una ejecución momentánea, o una tarea cíclica, usamos watch:

![\1](/Attachments/Pasted%20image%2020250601174900.png)
1. _watch es una utilidad en Linux que nos permite ejecutar repetidamente un comando en intervalos de tiempo, puedes verlo como un bucle por tiempo de un comando_
2. _-n 1 es la flag que nos permite determinar el intervalo de actualización del comando, en este caso, deseamos que cada segundo se ejecute nuevamente.
3. _"ps aux | grep echo.sh" es el comando que deseamos ejecutar de manera cíclica (En este caso agregamos "grep echo.sh" para filtrar solamente por el proceso de nuestro interés) _

![\1](/Attachments/Pasted%20image%2020250601174914.png)
_Vemos que el proceso de root aparece y desaparece, lo que quiere decir que se esta lanzando a intervalos_.

>Lo anterior nos permitió escalar privilegios en una maquina, también nos hubiera servido "Top" o "Htop", pero, watch es mucho mas funcional. Podemos también:

```bash
watch -d 'tail -n 20 /var/log/syslog'
```
_Ver cambios de un archivo en tiempo real_
_Usamos "tail -n 20" para leer las ultimas 20 líneas del archivo /var/log/syslog (Archivo de registro central), esto por defecto se ejecutara cada 2 segundos con watch, y con "-d" marcamos las diferencias que se dan entre intervalos_

```bash
watch -n 1 'free -h'
```
_Monitorizar el uso de memoria en el sistema_
_El comando "free -h" lee y procesa el archivo virtual del kernel donde se encuentran las estadísticas de memoria (/proc/meminfo). Con "watch  -n 1" lo ejecutamos cada segundo_

```bash
watch -d 'df -h | grep -v tmpfs'
```
_Monitorizar el espacio en el disco
El comando "df -h" a grandes rasgos, nos muestra el espacio actual en el disco. Al tratar su Output con "grep -v tmfs" filtramos y excluimos de la respuesta general los sistemas de archivos temporales. Para finalmente ejecutarlo por defecto cada 2 segundos con watch y resaltando sus diferencias con "-d".

```bash
watch -d 'who'
```
_Ver usuarios conectados en tiempo real_
_La herramienta "Who" lee e interpreta el registro de sesiones activas (/var/run/utmp/). Lo anterior lo hacemos de manera cíclica con "watch" y diferenciando respuestas con "-d"_
_En sistemas mas modernos lo mejor seria usar "watch -d  'loginctl list-users'_"

>Y hasta monitorizar archivos abiertos en un proceso:

```bash
watch -n 1 'lsof -p $(pgrep python)'
```
1. _Con "pgrep Python" Listamos todos los procesos en /proc del binario "Python"_
2. _El stdout del anterior comando (Que nos da los PIDs de los procesos de Python) se lo pasamos como argumento al comando "lsof -p", que con ello, lista los archivos abiertos por el proceso.
3. Por ultimo todo lo ejecutamos en bucles de 1 segundo con "Watch"_

![\1](/Attachments/Pasted%20image%2020250613030039.png)

![\1](/Attachments/Pasted%20image%2020250613030058.png)