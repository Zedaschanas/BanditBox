
--------------

>En Linux, los procesos se pueden definir como instancias en ejecución de un programa en especial. Saber listarlos y entenderlos se vuelve primordial al intentar escalar privilegios en una maquina.
>Para el tratamiento de procesos hay varias herramientas, pero, por ahora, solo para listarlos nos podemos centrar en el comando "ps".

>Los procesos se encuentran organizados y diferenciados en las carpetas /proc/. Lo que hace el comando "ps" es buscar el directorio asociado a un proceso y extraer información del mismo. Es decir, busca el proceso --> /proc/PID_del_proceso, y lista la información existente en sus directorios --> /proc/PID_del_proceso/status (para verificar su estado) o /proc/PID_del_proceso/cmdline (para verificar el comando ejecutado) o /proc/PID_del_proceso/meminfo (Para calcular su uso en memoria).
>Un ejemplo sencillo:

![\1](/Attachments/Pasted%20image%2020250601172111.png)
1. _"ps" nos muestra información sobre los procesos en ejecución "Process status"_
2. _"a" nos muestra los procesos de todos los usuarios, no solo del propio_
3. _"u" nos permite ver mas información; como el usuario dueño de un proceso o el uso de memoria_
4. _"x" nos muestra todos los procesos que operan por separado de la terminal; como demonios o servicios en segundo plano._

>La información que obtenemos del listado de procesos termina siendo invaluable cuando encontramos un proceso particular, fuera de lo común...
>En el caso anterior descubrimos que el usuario ROOT ejecutaba de manera cíclica el comando /bin/bash sobre un archivo especial al que teníamos acceso.