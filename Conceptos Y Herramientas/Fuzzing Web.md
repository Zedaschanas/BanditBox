
----------

>El objetivo principal al hacer Fuzzing Web es encontrar rutas, archivos o parámetros en aplicaciones web, enviando peticiones masivas (Como una fuerza bruta) con cadenas o "Payloads" ya definidos en diccionarios.
>Se trata de analizar las respuestas que arroja un servidor sobre cada payload enviado, en búsqueda de cambios en el tamaño, código o contenido de la pagina web. Detectando así recursos o funciones que no deberían ser accesibles.

![Where Is My Web Shell](/Attachments/Where%20Is%20My%20Web%20Shell%207.png)
Hay muchas herramientas para enumerar directorios; [Dirb]([https://www.kali.org/tools/dirb/](https://www.kali.org/tools/dirb/) ), [Gobuster]([https://github.com/OJ/gobuster](https://github.com/OJ/gobuster) ), [Wfuzz]([https://www.kali.org/tools/wfuzz/](https://www.kali.org/tools/wfuzz/) ), Pero tal vez la más eficiente es [ffuf]([https://github.com/ffuf/ffuf](https://github.com/ffuf/ffuf) ), así que para usarla:

1. _-c Para que se vea bonito con colorcitos_ ╰(*°▽°*)╯
2. _-w Para especificar el diccionario que se probará en la url (uso un diccionario de [Seclists](https://github.com/danielmiessler/SecLists) )_
3. _-t Para indicarle la cantidad de hilos (tareas múltiples) que deseo para la tarea a realizar, en este caso 200_
4. _-u Indicamos la web a atacar, con la palabra interna FUZZ le indicamos a la herramienta que parte de la url debe ser cambiada por las palabras del el diccionario._
5. _-e Indicamos diferentes terminaciones que deseamos agregar a cada iteración en el diccionario, mientras que las comillas vacías hacen referencia a probar la palabra sin ninguna terminación (ej: -e .txt,.php,”” Probará en la web: palabra.txt, palabra.php y palabra)_

>O si deseas usar Wfuzz:

![Trust](/Attachments/Trust%206.png)

1. _-c (Para que los códigos de estado en la respuesta tengan colorcitos)_ O.O
2. _-w (Para indicar la Wordlist o lista de palabras con la que Wfuzz aplicara fuerza Bruta, el Wordlist que usamos es de [Seclist](https://github.com/danielmiessler/SecLists_](https://github.com/danielmiessler/SecLists))
3. _--hl hide lines(Se usa para que en la respuesta de Wfuzz no aparezcan los resultados que en el código de la web tengan 368 líneas, en este caso, es para que no aparezcan algunos falsos positivos)_
4. _--hc hide code(Se usa para filtrar un código de estado no deseado en la respuesta. Ya que el 404 es un código de ERROR, lo filtramos para que no aparezca)_
5. _-u (Lo usamos para determinar la ip a examinar, en ella, al final se coloca la palabra clave FUZZ, ese mismo será el lugar donde Wfuzz probara todos los posibles directorios)_
6. _-t (Con este parámetro le informamos a Wfuzz cuantos hilos deseamos que sean usados para esta tarea, es decir, cuántas combinaciones de palabras va a probar al mismo tiempo, en este caso 20)_