
------------
>[CiberChef](https://gchq.github.io/CyberChef/) es una herramienta sumamente potente de procesamiento de datos creada por el servicio de inteligencia británico y de código abierto. Puedes verlo como una navaja suiza (permite extraer metadatos, decodificar archivos ofuscados o codificar y ofuscar payloads). Cada operación es un modulo en JavaScript que funciona 100% del lado del cliente (no tiene backend). Con su función MAGIC se puede hacer una detección automática de algoritmos, como en este caso.

![\1](Attachments/Pasted%20image%2020250510155339.png)

>Ciber Chef tiene una cadena de procesamiento de datos relativamente fácil de usar:
1. _Panel de entrada --> Input_
2. _Operaciones a realizar sobre el Input --> Recipe_
3. Salida del Input ya procesado --> Output

>Ejemplo pequeñito...
>Codificamos una cadena en Hex, luego, en Base 64:

![\1](Attachments/Pasted%20image%2020250611042402.png)
_Con el parámetro -n quitamos el salto de linea que añade "Echo" a la cadena. Luego, con "xxd" codificamos la cadena._

>Luego, codificamos nuevamente la cadena, ahora con base64:

![\1](Attachments/Pasted%20image%2020250611044309.png)

>En Ciber Chef, introducimos la cadena codificada en "Input", para luego tratarla en el apartado Recipe:

![\1](Attachments/Pasted%20image%2020250611045119.png)
_Desde la zona izquierda agregamos las capas de decodificado, y la cadena es perfectamente legible (Mas o menos) en el apartado Output_

