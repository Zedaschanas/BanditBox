
-----------------

>Nmap es de las herramientas mas poderosas en el escaneo de puertos, automatiza todo el proceso de escaneo, permitiéndonos encontrar con total eficiencia puertos abiertos en un servidor, Identificando servicios de tecnologías y versiones.

>Uno de los escaneos mas usuales al hacer maquinas CTF (En entornos controlados) es o se parece a:

![\1](Attachments/Pasted%20image%2020250611163341.png)
1. _-- min-rate 5000 (quiero tramitar mínimo 5000 paquetes por segundo) esto para que el escaneo vaya con bastante agilidad._
2. _-n (no deseo que nmap haga una resolución DNS automática)
3. _-p- (quiero escanear los 65535 puertos del sistema, no los 1000 más comunes, como normalmente hace nmap)._
4. _-Pn (Omite el descubrimiento de Host)_
5. _-sVC (Aquí se concatena el parámetro -sV que realiza un reconocimiento de versiones de servicios, y el parámetro -sC que aplica unos cuantos scripts de reconocimiento propios de nmap_

>El escaneo anterior nos da un resultado bastante completo y rápido sobre los puertos abiertos de una maquina y sus servicios. Pero, no engloba prácticamente nada de todas las funciones que tiene la herramienta, haciendo algunos ejemplos pequeños:

![\1](Attachments/Pasted%20image%2020250611054047.png)
_De esta manera, con el parámetro -S falsificamos la ip de origen (Todos los paquetes parecen venir de la ip 172.17.0.1). Con -e, determinamos la interfaz de red a la que pertenecen las IPs_

![\1](Attachments/Pasted%20image%2020250611164123.png)
1. _Con el parámetro -sS evitamos que se complete el estandarizado "Thee-Way Handshake", es decir, no se completa la conexión de paquetes estandarizada para determinar que un puerto esta abierto, esto hace el escaneo LIGERAMENTE mas sigiloso y un poco mas rápido.
2. _-n para no hacer resolución DNS
3. _Con -D creamos IPs señuelos, en donde se mezclara el trafico real de nuestra ip hacia la maquina victima con el de los señuelos "172.17.0.4" y "172.17.0.3". Es decir, el objetivo vera ataques desde múltiples IPs simultáneamente, haciendo mas difícil el reconocimiento.

![\1](Attachments/Pasted%20image%2020250611171131.png)
_Este comando es ultra sigiloso, perfecto para entornos con detección de escaneo automática_
1. 1. _Con el parámetro -sS evitamos que se complete el estandarizado "Thee-Way Handshake", es decir, no se completa la conexión de paquetes estandarizada para determinar que un puerto esta abierto, esto hace el escaneo LIGERAMENTE mas sigiloso y un poco mas rápido.
2.  _-Pn (Omite el descubrimiento de Host)_
3. _-n para no hacer resolución DNS
4. Con -T 1, seleccionamos la velocidad de envió de paquetes en modo "ultra paranoico", por defecto en nmap, es -T 3.
5. Con "--scan-delay 5s" añadimos retardos aleatorios de 5 segundos entre paquetes enviados
6. "--max-retries 1" nos permite estandarizar como máximo un reintento por cada puerto que no este abierto.
7. De la misma manera que en el ejemplo anterior, con "-D" creamos IPs señuelos, en este caso "RND:5,ME" nos ayuda a crear 5 IPs aleatorias aparte de la nuestra, mezclando todo el trafico real con el ilegitimo.
8. -f fragmenta los paquetes enviados
9. Con "-g 53" usamos el puerto 53 de DNS para simular trafico legitimo 
10. Por ultimo, con --data-length 24 añadimos relleno a los paquetes enviados para evitar que sea demasiado notorio que provienen de Nmap

>Otros comando bastante útiles que encontré, pueden ser:

```bash
torsocks nmap -sT -Pn -n --proxy socks4://127.0.0.1:9050 172.17.0.2
```
_Para realizar el escaneo desde proxies tor (Para instalar torsocks con un "sudo apt install" debería ser suficiente), usamos el parámetro -sT para no realizar automáticamente un "SYN scan", si no un "TCP Connect scan" que es mas compatible con TOR_.

```bash
torsocks nmap -sT -Pn -n --scan-delay 10s --proxy socks5://127.0.0.1:9050 172.17.0.2
```
_Se podría considerar como un modo ultra paranoico usando torsocks_

```bash
sudo nmap -S 172.17.0.5 --spoof-mac 0 -e Docker0 -Pn 172.17.0.2
```
_De esta manera no solo Falsificamos la IP de origen del escaneo con "-S", si no que también con "--spoof-mac" falsificamos la dirección MAC_

```bash
proxychains nmap -sT -Pn -n -T2 172.17.0.2
```
_Escaneo desde Proxychains (Para ello lo mejor seria definir VARIOS proxies en /etc/proxychains.conf)_

```bash
sudo nmap -sU --data-length 24 --badsum -Pn -f 172.17.0.2
```
_Escaneamos puertos que utilizan el protocolo UDP no TCP, y con --badsum jalamos de algunos checksums invalidos para evadir detecciones_

```bash
nmap -PE -PP -PM -Pn --disable-arp-ping --packet-trace 172.17.0.2
```
_Si ni TCP o UDP Funcionan podemos escanear por ICMP_

>Esas son solo algunas de todas las flag y funciones que nos da [Nmap](https://nmap.org/book/man.html)

