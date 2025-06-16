
------------
>Maquinita fácil para pasar un rato agradable :3, así que empezamos:

![BorazuwarahCTF](Attachments/BorazuwarahCTF.png)

>Hacemos un Ping para verificar la conexión con la maquina

![BorazuwarahCTF](Attachments/BorazuwarahCTF%201.png)
Con esto en pocas palabras; enviamos tramas ICMP “Internet Control Message Protocol” tipo (Echo Request) a la ip victima, esta misma, al estar en funcionamiento, revisa las cabeceras del paquete para verificar que es para ella, y responde con un (Echo Reply).)

1. _Podemos ver el orden de estas tramas ICMP en el apartado “icmp_seq=”,
2. _Con el valor de “ttl=” podemos ver el número máximo de saltos que puede dar un paquete antes de descartarse (Por lo general funciona para determinar el sistema operativo víctima)
3. _Con el valor “time=” podemos ver el tiempo entre el “Echo Request” y el “Echo Reply”)

>Procedemos a hacer un escaneo fuerte con nmap para identificar puertos abiertos

![BorazuwarahCTF](Attachments/BorazuwarahCTF%202.png)
1. _-- min-rate 5000 (quiero tramitar mínimo 5000 paquetes por segundo) esto para que el escaneo vaya con bastante agilidad._
2. _-n (no deseo que nmap haga una resolución DNS automática) 
3. _-p- (quiero escanear los 65535 puertos del sistema, no los 1000 más comunes, como normalmente hace nmap)._
4. _-Pn (Omite el descubrimiento de Host)_
5. _-sV (realiza un reconocimiento de versiones de servicios)_

>Bien, vemos el puerto 22 con OpenSSH y el 80 con un servicio de Apache, sus versiones son relativamente actualizadas, así que no buscaremos vulnerabilidades desde aquí, empezamos profundizando un poco por el puerto 80:

![BorazuwarahCTF](Attachments/BorazuwarahCTF%203.png)

>Vemos una imagen del creador de la maquina dándonos cariño, por lo general, podríamos proceder por fuzzear Directorios o inspeccionar el código fuente, pero en los CTF y maquinas vulnerables es muy usual que estas imágenes tengan información, así que analicémosla un poco:

![BorazuwarahCTF](Attachments/BorazuwarahCTF%204.png)

>Con el comando "Wget" nos descargamos la imagen a nuestra maquina y ahora con "exiftool" podemos analizar los metadatos:

![BorazuwarahCTF](Attachments/BorazuwarahCTF%205.png)

>Vemos que aparece en texto plano un usuario en la descripción, para obtener un poco mas de información podemos sacar los strings legibles de la imagen con el comando “strings”:

![BorazuwarahCTF](Attachments/BorazuwarahCTF%206.png)

>Vemos que fueron metadatos incluidos en la imagen con la misma herramienta exiftool (que sirve para leer y escribir metadatos en imágenes). Así que probamos fuerza bruta ha la contraseña en el servicio SSH:

![BorazuwarahCTF](Attachments/BorazuwarahCTF%207.png)
1. _-l (este parámetro se usa determinar el usuario que ya tenemos, si no tenemos un usuario, al colocar “-L” podemos incluir una wordlist para atacarlos también)_
2. _-P (para determinar, una wordlist con posibles contraseñas, si tuviéramos una contraseña y quisiéramos probar usuarios, en este punto colocamos “-p” y la contraseña)_
3. _ssh:// (para determinar el servicio a atacar, puede ser http// o ftp://, en este caso ssh://)_
4. _-t (indico que deseo emplear 64 hilos, para agilizar el escaneo)_

>Ya con el usuario y la contraseña, podemos ingresar a la maquina por ssh

![BorazuwarahCTF](Attachments/BorazuwarahCTF%208.png)

>Y particularmente, al listar privilegios en la maquina, vemos que tenemos permisos para todo asi que simplemente usando un “sudo su” y digitando nuestra contraseña como usuario borazuwarah:

![BorazuwarahCTF](Attachments/BorazuwarahCTF%209.png)