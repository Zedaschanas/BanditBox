
--------
>Hoy vamos con otra maquina muy sencillita, para pasar el rato y aprender un poco de paso.

**![Obsession](Attachments/Obsession.png)**

>Empezamos por levantarla:

![Obsession](Attachments/Obsession%201.png)

>Después, verificamos que tenemos conexión con la maquina. Usamos el comando ping

![Obsession](Attachments/Obsession%202.png)
_(Con esto en pocas palabras; enviamos tramas ICMP “Internet Control Message Protocol” tipo (Echo Request) a la ip victima, esta misma, al estar en funcionamiento, revisa las cabeceras del paquete para verificar que es para ella, y responde con un (Echo Reply).)

1. _Podemos ver el orden de estas tramas ICMP en el apartado “icmp_seq=”,
2. _Con el valor de “ttl=” podemos ver el número máximo de saltos que puede dar un paquete antes de descartarse (Por lo general funciona para determinar el sistema operativo víctima)
3. _Con el valor “time=” podemos ver el tiempo entre el “Echo Request” y el “Echo Reply”)

> Vamos al primer escaneo:

![Obsession](Attachments/Obsession%203.png)
1. _-- min-rate 5000 (quiero tramitar mínimo 5000 paquetes por segundo) esto para que el escaneo vaya con bastante agilidad._
2. _-n (no deseo que nmap haga una resolución DNS automática)
3. _-p- (quiero escanear los 65535 puertos del sistema, no los 1000 más comunes, como normalmente hace nmap)._
4. _-Pn (Omite el descubrimiento de Host)_
5. _-sV (realiza un reconocimiento de versiones de servicios)_

>Encontramos 3 servicios corriendo en versiones relativamente actuales, así que, en primera medida, vamos a la web.

![Obsession](Attachments/Obsession%204.png)

>Vemos una pagina muy simple, ningún link nos direcciona a ningún lado, excepto el del GitHub de su creador. Hay un panel de datos, que realmente no tramita información, así que procedemos a analizar el código fuente de la página:

![Obsession](Attachments/Obsession%205.png)

>Encontramos un muy pero muy sutil mensaje, ya al saber que reutiliza el usuario "russoski" para todos los servicios, atacamos con fuerza bruta:

![Obsession](Attachments/Obsession%206.png)
1. _-l (este parámetro se usa determinar el usuario que ya tenemos, si no hubiéramos tenido un usuario, al colocar “-L” podemos incluir una wordlist para probar usuarios)_
2. _-P (para determinar una wordlist con posibles contraseñas, si tuviéramos una contraseña y quisiéramos probar usuarios, en este punto colocamos “-p” y la contraseña)_
3. _ssh:// (para determinar el servicio a atacar, puede ser http// o ftp://)_
4. _-t (indico que deseo emplear 64 hilos, para agilizar el escaneo)_

>Una vez encontradas las credenciales, ingresamos:

![Obsession](Attachments/Obsession%207.png)

>Al listar privilegios para el usuario "russoski", vemos que puede ejecutar “VIM” Como cualquier usuario

![Obsession](Attachments/Obsession%208.png)
_Ejecutamos el binario setuid (que siempre se ejecuta como root)“SUDO”, con la flag “-l”. El binario busca los privilegios que el usuario actual tiene descritos en el archivo /etc/sudoers y los muestra por pantalla (Si aplica)_

1. _(root) Permite ejecutar el binario como el usuario mas privilegiado_
2. _NOPASSWD Nos indica que no es necesario ingresar contraseña para ejecutar el binario de manera privilegiada_
3. _/usr/bin/vim Binario sobre el que tenemos privilegios (cuya ruta está definida arriba en “secure_path”_

>Esto es toda una bendición, ya que “vim” es mucho mas que un editor de texto, es tan potente, que permite la ejecución de comandos, pero, si lo ejecutamos como SUDO, los comandos se ejecutaran con el contexto del usuario root y no como el usuario "russoski", así que:

![Obsession](Attachments/Obsession%209.png)

![Obsession](Attachments/Obsession%2010.png)

La máquina es nuestra :3

_//nota: el comando rm -rf /* ejecutado como administrador borra todos los archivos del sistema, es un pequeño guiño al ganar acceso como root a una máquina, NO LO EJECUTES//_