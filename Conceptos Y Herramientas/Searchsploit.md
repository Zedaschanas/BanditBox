
-----------------

>Searchsploit es una herramienta de terminal que filtra y busca entre todos los exploits que se encuentran en [ExploitDB](https://www.exploit-db.com/). 
>En el paquete preinstalado en Kali "exploitdb" existe un archivo files_exploits.csv en donde se encuentran los metadatos de TODOS los exploits.

![\1](Attachments/Pasted%20image%2020250504142751.png)

>La herramienta Searchsploit filtra por todos ellos buscando coincidencias por sistema operativo, descripción o nombre del servicio, hasta encontrar una coincidencia para una vulnerabilidad que estemos buscando.

![\1](Attachments/Pasted%20image%2020250612035226.png)

>Con el parámetro -m (mirror) creamos una copia en el directorio de trabajo actual del exploit que deseamos. Searchsploit copia este archivo desde una ruta en particular del sistema en la que se almacenan los exploits del paquete "ExploitDB":

![\1](Attachments/Pasted%20image%2020250612171650.png)

>También, podemos excluir respuestas no deseadas con la flag "--exclude", filtrando por cadenas que representan una explotación que no nos interesa:

![\1](Attachments/Pasted%20image%2020250612172235.png)
_De esta manera excluimos cualquier exploit ".txt" buscando solamente scripts automatizados_

>Una alternativa si "Searchsploit" nos es infructuoso, seria buscar directamente en [Exploitdb](https://www.exploit-db.com), [GitHub](https://github.com/search?q=CVE-2024-6387&type=repositories) o [rapid7](https://www.rapid7.com/db/vulnerabilities/openbsd-openssh-cve-2024-6387/). También, es eficaz ocasionalmente usar el motor de búsqueda [sploitus](https://sploitus.com/) o [Vulmon](https://vulmon.com/):

![\1](Attachments/Pasted%20image%2020250612172926.png)
_En el tenemos exploits de fuentes mucho mas variadas_

![\1](Attachments/Pasted%20image%2020250612173504.png)
_La plataforma no contiene directamente los exploits, pero, por lo general en sus "referencias" sobre cada vulnerabilidad hay webs que explican paso a paso una explotación o que contienen una prueba de concepto con el exploit_