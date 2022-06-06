---
layout: post
title: Montar unidades SMB o CIFS en Linux con mount o fstab
date: 2021-02-16 10:46:27.000000000 +01:00
type: post
parent_id: '0'
published: true
password: ''
status: publish
categories:
- Sin categoría
tags:
- cifs
- linux
- samba
---
Hace relativamente poco creé un pequeño servidor samba en mi casa con la idea de poder visualizar y publicar archivos en un único lugar conjunto (la idea de tener mi propia nube personal me abruma desde hace años, muchos lo saben). Pero el sistema que se usa generalmente en Linux y en Windows es distinto, Windows monta la unidad en un nuevo disco (el Z: por defecto) mientras que en Linux debes acceder como ubicación remota. Esto genera ciertos problemas, especialmente a la hora de acceder desde algunas aplicaciones que no aceptan archivos remotos.

Por eso decidí indagar en el tema y, a decir verdad, pocas cosas me funcionaban. Hasta le pedí ayuda a una profesora mía. Todo eso, junto con las consultas incesantes a los manuales del sistema dieron sus frutos. Al fin y al cabo, no estaría escribiendo esto si no lo hubiese conseguido, ¿no es así?

## Montaje manual (mount)

Primero de todo debía encontrar la forma de montar la unidad sin explotar, luego me encargaría de hacer que el sistema la monte automáticamente. Recopilando la información de varios blogs y foros, junto con los incontables fallos que tuve, encontré la solución a toda la parafernalia. Voy a indicar el comando interminable que debe usarse y a explicar el porqué de cada cosa.

```terminl
sudo mount -t cifs //server/data /local -o username=sambauser,password=paswd,iocharset=utf8,uid=localuser,gid=localgroup,forceuid,forcegid,cifsacl
```

### Qué hace cada cosa

<ul>
<li><strong>sudo:</strong> Indicamos que el comando debe ejecutarse como root. Obviamente para ello nuestro usuario tendrá que estar en la lista sudousers.</li>
<li><strong>mount:</strong> El comando a ejecutar. Mount es usado principalmente para montar y visualizar las particiones del sistema.</li>
<li><strong>-t cifs:</strong> Le indicamos al comando mount que debe utilizar el sistema de ficheros CIFS. Este es el nombre que usa Windows para el protocolo SMB. Según los manuales también se puede usar <strong>smbfs</strong>, pero este nunca me ha funcionado.</li>
<li><strong>//server/data:</strong> Debemos indicar la dirección IP del servidor al que nos queremos conectar usando primero dos barras ( <code>//</code> ). Si nos queremos conectar a un directorio específico lo indicaremos usando otra barra y la dirección.</li>
<li><strong>/local:</strong> Indicamos en qué directorio local queremos que se monte, puede estar colgando del directorio raíz o de dónde sea, pero siempre indicar una ruta absoluta. Separaremos esta información de la del servidor con un espacio</li>
<li><strong>-o:</strong> Ahora viene lo tocho. Con -o avisamos a mount que vamos a especificar todos los requerimientos para el montaje. Debe colocarse todo sin espacios.
<ul>
<li><strong>username=sambauser:</strong> Cambiamos "sambauser" por <strong>el nombre de usuario de samba</strong> con el que nos vamos a conectar. No pasa nada si ese usuario no existe en el sistema.</li>
<li><strong>password=paswd:</strong> Cambiamos "paswd" por <strong>la contraseña del usuario samba</strong>. Como he dicho antes, no tiene por qué coincidir con alguna contraseña local. Tanto el usuario como la contraseña o el dominio (que no lo vamos a usar aquí), se pueden guardar en un archivo protegido (pero no cifrado que yo sepa) para evitar a los curiosos.</li>
<li><strong>iocharset=utf8: </strong>Con esto especificamos el tipo de codificación que tienen los archivos. Usando esto evitamos problemas de compatibilidad con otros ordenadores que estén conectados.</li>
<li><strong>uid=localuser:</strong> Cambiamos "localuser" por un usuario del sistema. Una de las guindas del pastel. Con este opción marcamos <strong>el usuario local</strong> que va a conectarse a samba. El sistema cogerá los permisos del usuario "sambauser" que hemos indicado arriba y sustituirá en el montaje los permisos de este dándoselos al usuario "localuser". </li>
<li><strong>gid=localgroup:</strong> Cambiamos "localgroup" por <strong>un grupo del sistema local</strong>. Al igual que la opción anterior, montará los ficheros y directorios poniendo a  "localgroup" los permisos que en samba le corresponden al grupo de "sambauser". </li>
<li><strong>forceuid,forcegid</strong>: Añadimos estas opciones cuando el usuario y el grupo del sistema no coinciden con el usuario y el grupo con el que queremos acceder a samba. </li>
<li><strong>cifsacl:</strong> Debemos marcar esta opción para asegurarnos que los permisos se añaden correctamente durante el montaje. Con esto no solo evitamos problemas de no poder escribir en ciertos archivos, sino también de no acceder a las carpetas a las que realmente no tenemos permisos.</li>
</ul>
</li>
</ul>

### Ejemplo práctico

En teoría, con toda esta parafernalia, ya lo tenemos listo. Por si a alguien le ha petado una neurona voy a indicar un ejemplo práctico.:

```terminal
sudo mount -t cifs //192.168.0.20/data /home/crotofroto/Datos -o username=owo,password=fakepassword,iocharset=utf8,uid=crotofroto,gid=crotofroto,forceuid,forcegid,cifsacl
```

Una vez nada de esto nos dé error significa que ya podemos montar la unidad samba en el sistema como si de un directorio más se tratase.

Se eres inconformista no te querrás quedar aquí, porque tras cada inicio de sistema tendrás que ejecutar el comando para que funcione. Y sí, podrías crear un script y añadirlo al crontab para que se monte siempre que enciendas el ordenador, pero hay una opción mucho mejor.

## Montaje automático (fstab)

El fichero /etc/fstab sirve para mostrar y editar los montajes del sistema. Dependiendo del nuestro, estará más o menos lleno. Lo importante es no tocar nada de lo que hay ya escrito, porque podríamos hacer que el ordenador no se vuelva a encender.

Ahora que ya tienes el miedo en el cuerpo, asegúrate de empezar a editar desde abajo añadiendo la siguiente línea:

```
//server/data    /local     cifs    username=sambauser,password=paswd,iocharset=utf8,uid=localuser,gid=localgroup,forceuid,forcegid,cifsacl,noauto,x-systemd.automount,_netdev 0 0
```

Puedes deducir que tiene los mismos datos que el comando mount solo que con una estructura distinta. Es importante que lo coloques toda tu configuración en ese orden y con el mismo espaciado para evitar problemas.

También podrás ver que al final tiene cinco parámetros nuevos.

### Qué hace cada cosa

<ul>
<li><strong>noauto,x-systemd.automount</strong>: Estas dos opciones suelen usarse cuando se tratan de particiones grandes. La primera indica que no se monte automáticamente al arrancar el sistema, esto se hace porque puede ralentizar significativamente el inicio. La segunda opción le dice al kernel que únicamente monte la partición cuando se va a acceder a ella por primera vez. De forma que para nosotros se sigue montando automáticamente pero sin ralentizar el sistema. La segunda opción depende directamente del demonio systemd que, por suerte o por desgracia, se encuentra en casi todos los sistemas Linux más utilizados.</li>
<li><strong>_netdev:</strong> Por inventarme algo vamos a traducirlo como "network device". Esta opción indica que la partición depende directamente de la red, por lo que no debe montarse si no está la red preparada para comunicarse con otros dispositivos y así evitar errores.</li>
<li><strong>0 0:</strong> Tu pon los dos ceros que sino no va. El porqué se colocan no forma parte de la explicación de esta entrada.</li>
</ul>

## Más información

Si tienes más dudas o errores o quieres colocar más opciones de montaje, te facilito aquí la wiki de ArchLinux, la cual me ha ayudado enormemente a poder solucionar todos los errores que me he ido encontrando y cuenta con muy buenas explicaciones tanto en inglés como en español.

Aunque uses un sistema Debian, todo lo aquí mencionado me ha servido tanto en Manjaro como en Ubuntu.</p>
<p><!-- /wp:paragraph --></p>
<p><!-- wp:paragraph --></p>
<p>Información sobre samba: <a href="https://wiki.archlinux.org/index.php/samba" target="_blank">https://wiki.archlinux.org/index.php/samba</a>

Información sobre fstab: <a href="https://wiki.archlinux.org/index.php/Fstab_(Espa%C3%B1ol)" target="_blank">https://wiki.archlinux.org/index.php/Fstab_(Espa%C3%B1ol)</a>
