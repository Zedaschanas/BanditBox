
-------------

>Whatweb es una herramienta de reconocimiento web creada en Ruby. Esta hecha para identificar tecnologías web (Es capaz de detectar Frameworks, plugins, CMS, etc).
>Por lo general, mediante una petición GET, recolecta información de toda la respuesta (Cookies, código HTML, encabezados), para que luego, el motor de whatweb con REGEX (expresiones regulares) y sus propios plugins (scripts) de Ruby analicen la respuesta en búsqueda de particularidades propias de tecnologías (Como wp-content para WordPress, X-Powered-By para php o /templates/Joomla para Joomla).

![\1](Attachments/Pasted%20image%2020250506152128.png)

>Al encontrar coincidencias. usa sus propios plugins (scripts) de Ruby para identificar las 
>tecnologías.

![\1](Attachments/Pasted%20image%2020250506154219.png)_
_Ruta de los scripts de Ruby y su cantidad_

>Whatweb nos permite modificar la agresividad del escaneo, ajustar hilos y hasta usar un proxy.
>Un escaneo bastante potente seria:

```bash
whatweb --no-errors -a 3 --max-threads 50 --open-timeout 5 --read-timeout 5 --follow-redirect=neve http://172.17.0.2
```
1. _Con "--no-errors" evitamos la espera de respuestas validas, acelerando el escaneo_
2. _"-a 3" Nos da el máximo grado de agresividad que permite Whatweb_
3. "--max-threads 50" nos permite ejecutar la tarea en 50 hilos diferentes para potenciar su velocidad
4. Con "--open-timeout 5" Cerramos conexiones TCP tras 5 seg
5. Con "--read-timeout" limitamos el tiempo de espera de respuestas HTTP
6. Por ultimo con --follow-redirect=never" Omitimos cualquier redirección y nos centramos SOLAMENTE en la url proporcionada

![\1](Attachments/Pasted%20image%2020250614012608.png)

>O un escaneo mas sigiloso seria:

```bash
whatweb -a 1 --max-threads 2 --user-agent "Mozilla/5.0" --wait=5 http:172.17.0.2
```
_En esta ocasión seleccionamos el nivel de agresividad 1 (Al mínimo), modificamos los hilos para realizar solamente 2 tareas en paralelo, creamos un "User Agent" de un navegador (Para simular trafico normal) y añadimos un "--wait=5" para esperar 5 segundos entre cada petición_

![\1](Attachments/Pasted%20image%2020250614014312.png)

>En realidad, en entornos controlados, la diferencia entre un escaneo y otro es imperceptible, pero en general, Whatweb funciona muy buen para enumerar tecnologías web en CTFs.