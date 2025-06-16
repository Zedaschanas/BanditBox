
-----------------

>Para verificar la conexión con un Host hay infinidad de herramientas, en realidad la mas usada en CTFs es Ping, y realmente es la mas fiable, probablemente para un entorno controlado no necesitemos nada mas, su uso mas básico es:

![nodeclimb](/Attachments/nodeclimb%202.png)
_(Con esto en pocas palabras; enviamos tramas ICMP “Internet Control Message Protocol” tipo (Echo Request) a la ip victima, esta misma, al estar en funcionamiento, revisa las cabeceras del paquete para determinar que es para ella, y responde con un (Echo Reply).)

1. _Podemos ver el orden de estas tramas ICMP en el apartado “icmp_seq=”,
2. _Con el valor de “ttl=” podemos ver el número máximo de saltos que puede dar un paquete antes de descartarse (Por lo general funciona para determinar el sistema operativo víctima)
3. _Con el valor “time=” podemos ver el tiempo entre el “Echo Request” y el “Echo Reply”)

>Aunque en realidad, al verificar la conexión por ICMP, Nmap también nos puede ayudar:

![\1](/Attachments/Pasted%20image%2020250613013705.png)
_Con -PE Enviamos paquetes "Echo Request" igual que ping y con -sn escaneamos solamente el Host y no sus puertos.

>Con Nmap también podemos tratar con otros tipos de ICMP si el "Echo Request" esta bloqueado:

![\1](/Attachments/Pasted%20image%2020250613013956.png)
_El -PP para ICMP Timestamp y el -PM para ICMP Netmask_

>Si ICMP no funciona para verificar la conexión o esta bloqueado, tenemos "Arping" para probar conexiones por ARP:

![\1](/Attachments/Pasted%20image%2020250613014510.png)

>O con Nmap podemos escanear solo el Host, sin escaneo de puertos:

![\1](/Attachments/Pasted%20image%2020250613015240.png)
_Nmap utiliza varios protocolos al verificar la conexión con un Host, así que esta manera se puede considerar relativamente fiable_

>Podemos hacer la verificación de conexión también podríamos usar "traceroute":

```bash
traceroute 192.168.1.1
```
_Traceroute envía paquetes UDP a puertos que no tienen ningún servicio, la maquina victima responde con un ICMP "Port Unreachable" y así verifica la conexión_

>Aunque se sale un poco de la temática simple de "Verificar conexión", tal vez la herramienta mas potente seria hping3 (Per defecto instalada en Kali), nos permite:

![\1](/Attachments/Pasted%20image%2020250613020645.png)
_Hacer una verificación de conexión evitando detecciones o reglas de firewall_
1. _Con -S enviamos paquetes SYN a la maquina victima_
2. Con -f fragmentamos los paquetes enviados
3. Con "--rand-source" enviamos paquetes desde IPs aleatorias, no solo desde la atacante
4. Con -t modificamos el "Time To Live" simulando el de una maquina Windows

>O:

![\1](/Attachments/Pasted%20image%2020250613021644.png)
_La opción "--flood" nos permite enviar paquetes sin esperar respuesta, siendo potencialmente devastador, a modo de traza claro, ya que en la practica en realidad no nos sirve de mucho :( _

>O con la flag -6 también podríamos enviar paquetes por IPv6, en si, una herramienta muy completa.

