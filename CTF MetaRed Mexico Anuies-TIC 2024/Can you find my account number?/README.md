# Desafío: Can you find my account number?
**Categoría:** Stego
**Fecha:** Octubre 2024  

## Descripción
For security reasons I have saved my account number in a GIF file. Can you help me find it?

## Solución
El archivo provisto tiene extensión .mp4. Al revisarlo, los primeros bytes muestran la firma de un archivo PDF, mientras que la descripción indica que se ha guardado el número de la cuenta en un GIF. 
Al abrir el archivo se muestran un par de imágenes y un texto que habla sobre la ciberseguridad. Pero esta información no es relevante para resolver el desafío.

Los puntos clave para avanzar en la resolución se encuentran en los strings del archivo. Uno de ellos es un mensaje PGP encriptado:

![pgp_message](https://github.com/distro-hopper/CTF/blob/main/CTF%20MetaRed%20Mexico%20Anuies-TIC%202024/Can%20you%20find%20my%20account%20number%3F/media/pgp_message.png?raw=true)


El otro está relacionado con lo que indica la descripción, el número de la cuenta guardado en un GIF, aunque no parece haber algún GIF en el archivo:

![gif_miss](https://github.com/distro-hopper/CTF/blob/main/CTF%20MetaRed%20Mexico%20Anuies-TIC%202024/Can%20you%20find%20my%20account%20number%3F/media/gif_miss.png?raw=true)
(Este filtro busca magic numbers de archivos GIF: GIF86a, GIF89a)

La forma de ubicar el GIF dentro del archivo es reemplazando la "I" por "1":

![gif_match](https://github.com/distro-hopper/CTF/blob/main/CTF%20MetaRed%20Mexico%20Anuies-TIC%202024/Can%20you%20find%20my%20account%20number%3F/media/gif_match.png?raw=true)

Ahora, conociendo la existencia del GIF, lo que hay que hacer es corregir el byte y extraerlo. En este caso, uso hexedit para corregirlo:

![gif_fixing_hexedit](https://github.com/distro-hopper/CTF/blob/main/CTF%20MetaRed%20Mexico%20Anuies-TIC%202024/Can%20you%20find%20my%20account%20number%3F/media/gif_fixing_hexedit.png?raw=true)

Luego se extrae con grep, tomando varias líneas después del match con el patrón de la firma encontrada:

![gif_grep_output](https://github.com/distro-hopper/CTF/blob/main/CTF%20MetaRed%20Mexico%20Anuies-TIC%202024/Can%20you%20find%20my%20account%20number%3F/media/gif_grep_output.png?raw=true)


Obteniendo finalmente la imagen donde se muestra la información necesaria para desencriptar el mensaje PGP:

![file](https://github.com/distro-hopper/CTF/blob/main/CTF%20MetaRed%20Mexico%20Anuies-TIC%202024/Can%20you%20find%20my%20account%20number%3F/media/file.gif?raw=true)


Al desencriptar el mensaje con la key que muestra el gif, obtenemos la flag.

![pgp_flag](https://github.com/distro-hopper/CTF/blob/main/CTF%20MetaRed%20Mexico%20Anuies-TIC%202024/Can%20you%20find%20my%20account%20number%3F/media/pgp_flag.png?raw=true)


