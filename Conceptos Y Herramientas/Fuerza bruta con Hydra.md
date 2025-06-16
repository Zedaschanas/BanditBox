
-------------

>Hydra, es una de las herramientas de fuerza bruta mas usadas en el ámbito del pentesting y la resolución de CTFs. Como la usaremos muy seguido en entornos controlados, vale la pena ahondar un poco en ella...
>Hay bastantes mas herramientas de "Prueba De Credenciales", como Medusa o Ophcrack. Hydra destaca por su eficiencia, paralelizando de manera masiva tareas y siendo útil para una buena variedad de protocolos; como SSH, HTTP, FTP o SMB.

>Un escaneo del mas usual con Hydra seria sobre un servicio SSH, ya con un usuario, encontrando una contraseña:

![BreakMySSH](/Attachments/BreakMySSH%207.png)
1. _-l (este parámetro se usa para determinar el usuario que ya tenemos, si no tenemos un usuario, al colocar “-L” podemos incluir una wordlist para atacarlos también)_
2. _-P (para determinar, una wordlist con posibles contraseñas, si tuviéramos una contraseña y quisiéramos probar usuarios, en este punto colocamos “-p” y la contraseña)_
3. _ssh:// (para determinar el servicio a atacar)
4. _-t (indico que deseo emplear 64 hilos, para agilizar el escaneo)_

>Algunas técnicas complementarias interesantes:

```bash
hydra -l admin -x "4:6:aA1?*^" ssh://172.17.0.2
```
_Usamos REGEX con la flag "-x" Para determinar la creación de posibles contraseñas ("4:6" de 4 a 6 caracteres, "aA" con minúsculas y mayúsculas, "1" con números del 1 al 9, "?*^" con estos caracteres especiales) 

```bash
hydra http-post-form -L users.txt -P pass.txt \
"172.17.0.2/login:user=^USER^&pass=^PASS^:F=error" \
-s 443 -H "User-Agent: Mozilla/5.0 (Windows NT 10.0)" \
-H "X-Forwarded-For: 1.2.3.4" -t 3
```
_En este caso, atacando usuario y contraseña en un login http enviado por POST, con "-H" cambiamos el "user-agent" de forma dinámica, con -t 3 bajamos la cantidad de Hilos para evitar restricciones o alertas_

>Hasta podemos combinar Hydra con Hashcat (Usaríamos Hashcat como una especie de "Preprocesador"):

```bash
hashcat -a 6 -m 0 ?a?a?a?a --stdout | hydra -l admin -P - http-post-form "172.17.0.2/login:user=^USER^&pass=^PASS^"
```
_Con la primera parte "Hashcat -a 6 -m 0 ?a?a?a?a --stdout"_
1. _Con -a 6 generamos TODAS las combinaciones posibles para el patrón proporcionado_
2. _Usamos en modo raw MD5 "-m 0" para que no nos de un error :(
3. "?a?a?a?a" es una manera de representar una contraseña de 4 caracteres con TODOS los caracteres imprimibles ASCII
4. Con la ultima flag "--stdout" creamos un flujo continuo de contraseñas al "stdout" (Salida que usara el siguiente comando)
_Con la segunda parte "hydra -l admin -P - http-post-form"_
5. _Con -P - Leemos contraseñas desde el stdin_ (Desde el stdout del comando anterior)
6. "http-post-form" especifica un formulario por petición POST
7. Con la porción entre comillas enviamos por POST "POST /login HTTP/1.1...\n user=admin&pass=Aqui_probamos_las_contraseñas"

>Hydra es una de las herramientas de mas relevancia en CTFs, aunque, su uso siempre es a gusto del usuario, es decir, hay mas opciones bastante buenas, nos quedamos con la que mas nos gusta :3