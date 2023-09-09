---
layout: post
title: Instalar un sistema operativo a la Raspberry
date: 2019-09-26 18:36:01.000000000 +02:00
type: post
parent_id: '0'
published: true
password: ''
status: publish
categories:
- Apuntes SMR
tags:
- instalar
- kali linux
- kodi
- nextcloud
- noobs
- pi
- raspberry
- raspbian
- retropie
- sistema operativo
- tutorialx
---
Se conoce que la Raspberry es una herramienta excepcional para empezar en la informática, ya sea en programación, robótica... O incluso administración, porque si tienes pocos recursos puedes empezar a conocer los servicios, su funcionamiento y aplicarlos en tu casa gastando muy poco. Pero es importante que sepamos bien que sistema operativo instalar en la Raspberry para sacarle el máximo provecho.

## Qué necesitamos

Cuando vas a comprar una Raspberry hay que fijarse bien en qué modelo se compra, los modelos 1 y 2 están muy limitados y el 3 puede quedarse corto en según qué proyectos. También hay que fijarse en qué incluye la Raspberry que se compra, muchas veces es simplemente la placa y nada más, ni cable de alimentación siquiera. Al final, necesitaremos este material básico:

- **Placa base.** La propia Raspberry. Se debería mirar qué modelo comprar, según presupuesto y proyecto.
- **Cable de alimentación.** Usan un voltaje distinto a los cables comunes por lo que hay que estar atento, mejor comprar uno original. La Raspberry Pi 4 usa un USB-C, el resto un Micro-USB.
- **Tarjeta MicroSD.** Es la memoria principal de la Raspberry, donde estará el sistema. Puedes escoger el tamaño que quieras, luego puedes poner un USB para aumentar la memoria.
- **Periféricos de entrada.** Importante mínimo un teclado por USB, para introducir las instrucciones en terminal o navegar por la interfaz si conoces los atajos, mejor poner también un ratón si vas a usar entorno gráfico.
- **Periféricos de salida.** Tal vez no hagan falta tras configurarlo si lo vas a usar como servidor y conectarte por SSH, pero como mínimo en el primer inicio es lo mejor. También hay pantallas táctiles.

## Que sistema elegimos

Este es el punto fuerte de la entrada. Los sistemas operativos para la Raspberry son muy variados y están bastante especializados.

### Raspbian
![Logo de Debian](/assets/2019/09/debian-logo.png)

Es el sistema oficial de la Raspberry, una versión adaptada de Debian. Actualmente está en su versión Buster (Debian 10) y es el único sistema <b>oficial</b> para la Raspberry Pi.

Cuenta con una versión Lite, la cual es solo línea de comandos, para servidores. Y Desktop, con un escritorio y programas básicos como navegador, reproductor y <a href="https://www.minecraft.net/es-es/edition/pi/">su propio Minecraft</a>.

<a href="https://www.raspberrypi.org/downloads/raspbian/" target="_blank">Página oficial de descarga
</a>

### Kali Linux

![Logo Kali Linux](/assets/2019/09/kali-logo.png)

Otra distro basada en Debian. Es muy conocida por tener una infinidad de programas de monitorización de redes y hackeo. Tiene una versión ARM apta para Raspberry, la cual puede ser un buen kit de hackeo.

A mi parecer el mejor modelo para usarlo es la Raspberry Pi 4.

<a href="https://www.offensive-security.com/kali-linux-arm-images/#1493408272250-e17e9049-9ce8" target="_blank">Página oficial de descarga</a>

### Kodi

![Logo Kodi](/assets/2019/09/kodi-logo.png)

Es un centro multimedia de código libre. Se puede usar para añadir contenido a una TV antigua, sirviendo como almacenamiento de películas que guardes. Siempre ha habido mucha controversia por las extensiones de terceros que tiene, las cuales piratean series y canales de televisión.

<a href="https://libreelec.tv/downloads/raspberry/" target="_blank">Página oficial de descarga</a>

### RetroPi

![RetroPi logo](/assets/2019/09/retropie-logo.png)

Muy conocida por los melancólicos, aunque nunca me ha picado la curiosidad de probarlo. Por lo que sé incluye una buena cantidad de emuladores de 8 y 16 bits como SuperNintendo. Se pueden comprar carcasas de consolas y kits que incluyen mandos o una palanca de juegos.

<a href="https://retropie.org.uk/download/" target="_blank">Página oficial de descarga</a>

### Nextcloud Pi

![Nextcloud Pi logo](/assets/2019/09/nextcloud-logo.png)

Es una versión adaptada de Nextcloud a ARM. Es un sistema utilizado como servidor en la nube. Se puede usar dentro de una red LAN o configurar para acceder desde el exterior. Tiene herramientas de visualización de PDF, ofimática en línea, etiquetas, cuotas de disco, envío de correos automáticos... lo que se necesite para crear un servicio en la nube completo.

<a href="https://nextcloudpi.com/#debian-installation" target="_blank">Página oficial de descarga</a>

## Como instalar el sistema operativo

Para instalar cualquier sistema necesitaremos poner la tarjeta SD en un ordenador y, obviamente, tener descargado el sistema que queramos. Se usará una herramienta para "quemar" el sistema en la SD distinta a la usada para quemar CDs. Personalmente utilizo el programa <a href="https://www.balena.io/etcher/" target="_blank" rel="noopener noreferrer">Balena Etcher</a> en Ubuntu y <a href="https://rufus.ie/" target="_blank" rel="noopener noreferrer">Rufus</a> en Windows, ambos código libre y licencia Apache 2.0 y GPL 3 respectivamente.

Los pasos son sencillos e intuitivos.

![](/assets/2019/09/rufus-242x300.png)

Con Rufus solo se necesita indicar el dispositivo donde se quiere flashear el sistema operativo, seleccionar el fichero de este y listo. Es, junto a Etcher, el programa más rápido en hacerlo que he visto.

![](/assets/2019/09/capturaetcher.png)

Etcher está disponible para Windows, Linux y Mac. Tiene una interfaz muy agradable y en tres pasos lo hace todo. Las ISOs de Windows suelen dar fallos con este programa.

Tras instalar el sistema solo se conecta la microSD en su ranura y se conecta todo lo que se necesite, siendo el cable de alimentación lo último en conectar.

## NOOBS nos facilita la vida

![Logo de NOOBS](/assets/2019/09/noobs-e1569513936850-150x150.png)

A pesar de su nombre (siglas de <b>New Out Of the Box Software</b>, Nuevo Software Listo para Usar en inglés) es una herramienta muy útil y más sencilla de usar que las otras.

Por decirlo de alguna forma es un instalador de sistemas operativos.  Existen dos versiones de NOOBS. La primera permite instalar Raspbian y LibreELEC sin conexión a internet y el resto de sistemas disponibles los puede instalar si se conecta la Raspberry a Internet, la versión Lite necesita conexión para instalar todo.

Al descargar NOOBS no hay que flashear la SD, simplemente arrastras los archivos descomprimidos a la tarjeta y la conectas a la Raspberry. Entonces nos aparecerá un menú donde podremos elegir los sistemas a instalar (sí, pueden ser varios en Dual Boot). Se instalarán solos y elegiremos con cual iniciar por primera vez.

Ya está todo listo para comenzar a usar la Raspberry.