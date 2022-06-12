---
layout: post
title: Como instalar Adobe Reader en GNU/Linux de forma nativa
date: 2020-08-14 13:52:25.000000000 +02:00
type: post
parent_id: '0'
published: true
password: ''
status: publish
categories:
- Misceláneo
tags:
- Adobe
- ftp
- instalar
- libre
- linux
- okular
- Wine
---

La indiscutible fama de Adobe ha llevado a muchos a que alguna de sus herramientas sea imprescindible a la hora de trabajar en el ordenador. Ya sea con Adobe Photoshop o Adobe After Effects o el maravilloso Adobe XD (solo lo he puesto porque me hace gracia el nombre).

Aunque estas herramientas no están disponibles para GNU/Linux de forma nativa hay una, al menos, que sí lo está, aunque no sea algo muy comentado. Con unos sencillos pasos vamos a instalar Adobe Reader para Linux, esta aplicación nos permitirá visualizar PDFs y, aunque no tenga tantas funciones como su versión de Windows, sigue cumpliendo su función. Si se quiere usar la última versión siempre se puede optar por usar Wine y cruzar los dedos para que funcione.

## Obtención del .deb

Como siempre, evitaremos usar intermediarios y obtendremos el paquete de instalación directamente desde la página oficial de Adobe. Desde <a href="ftp://ftp.adobe.com/pub/adobe/reader/unix/" target="_blank">este enlace</a> podrás obtener todas las versiones para Linux de Adobe Reader. El enlace es un directorio FTP, puedes abrirlo desde Archivos o desde un navegador, aunque algunos de estos estén eliminando ya su compatibilidad con el protocolo FTP.

Si no puedes usar ninguno de estos métodos, aquí tienes directamente el comando que puedes utilizar para obtener la última versión disponible cuando se escribió el post:

```terminal
wget ftp://ftp.adobe.com/pub/adobe/reader/unix/9.x/9.5.5/enu/AdobeRdr9.5.5-1_i386linux_enu.deb
```

## Proceso de instalación

Adobe Reader solo está disponible en 32 bits para Linux, así que haremos unos pasos extra. Primero instalaremos la arquitectura de 32 bits. Con el siguiente comando se realiza la instalación y se actualizan los repositorios al mismo tiempo.

```terminal
sudo dpkg --add-architecture i386 &amp;&amp; sudo apt-get update
```

Tras esto, nos dirigimos al directorio donde hayamos descargado el programa y lo instalamos.

```terminal
sudo dpkg -i AdbeRdr9.5.5-1_i386linux_enu.deb
```

## Solución de errores

La instalación se detendrá por un fallo, le daremos la instrucción de continuar igualmente con el siguiente comando.

```terminal
sudo apt-get install -f
```

Ahora podremos ejecutar Adobe Reader con el comando <code><strong>acroread</strong></code>.

Probablemente de fallo en las librerías, por lo que las instalaremos manualmente.

```terminal
sudo apt-file search libxml2.so.2
sudo apt-get install libxml2:i386
```

Tras esto ya podremos ejecutar Adobe Reader de nuevo, lo podemos hacer mediante comandos o encontrarlo en las aplicaciones de Ubuntu.</p>

![](/assets/2020/08/imagen.png)

## Alternativas libres</h2>

Algunas funciones de la aplicación no funcionan y tiene un aspecto bastante antiguo. Aun así, la aplicación funciona a pesar de ser una versión descontinuada.

![](/assets/2020/08/imagen-1.png)

Existen opciones de código libre más actualizadas disponibles. Entre ellas se encuentra <a href="https://okular.kde.org/?site_locale=es" target="_blank">Okular</a>, el lector de PDFs de KDE.