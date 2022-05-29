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
<p><!-- wp:paragraph --></p>
<p>Se conoce que la Raspberry es una herramienta excepcional para empezar en la informática, ya sea en programación, robótica... O incluso administración, porque si tienes pocos recursos puedes empezar a conocer los servicios, su funcionamiento y aplicarlos en tu casa gastando muy poco. Por ejemplo, puedes usar una Raspberry como servidor Samba para compartir recursos entre todos los ordenadores y todas las cuentas de la casa, como fotos. Es importante que sepamos bien que sistema operativo instalar en la Raspberry para sacarle el máximo provecho.</p>
<p><!-- /wp:paragraph --><!-- wp:heading --></p>
<h2>Qué necesitamos</h2>
<p><!-- /wp:heading --><!-- wp:paragraph --></p>
<p>Cuando vas a comprar una Raspberry hay que fijarse bien en qué se compra, muchas veces es simplemente la placa y nada más, ni cargador ni nada, entonces es importante comprar un pack donde incluya el siguiente material mínimo o comprarlo por separado.</p>
<p><!-- /wp:paragraph --><!-- wp:list --></p>
<ul>
<li><strong>Placa base.</strong> La propia Raspberry. Se debería mirar qué modelo comprar, según presupuesto y proyecto.</li>
<li><strong>Cable de alimentación.</strong> Usan un voltaje distinto a los cables comunes por lo que hay que estar atento, mejor comprar uno original. La Raspberry Pi 4 usa un USB-C, el resto un Micro-USB.</li>
<li><strong>Tarjeta MicroSD.</strong> Es la memoria principal de la Raspberry, donde estará el sistema. Puedes escoger el tamaño que quieras, luego puedes poner un USB para aumentar la memoria.</li>
<li><strong>Periféricos de entrada.</strong> Importante mínimo un teclado por USB, para introducir las instrucciones en terminal o navegar por la interfaz si conoces los atajos, mejor poner también un ratón si vas a usar entorno gráfico.</li>
<li><strong>Periféricos de salida.</strong> Tal vez no hagan falta tras configurarlo todo pero como mínimo en el primer inicio es lo mejor. También hay pantallas táctiles.</li>
</ul>
<p><!-- /wp:list --><!-- wp:heading --></p>
<h2>Que sistema elegimos</h2>
<p><!-- /wp:heading --><!-- wp:paragraph --></p>
<p>Aquí viene un asunto importante. Dependiendo de <strong>para qué</strong> vayamos a usar nuestra Raspberry instalaremos un sistema u otro. Se tendrá que buscar cual es el que más se acopla a las necesidades pero aquí voy a dejar algunos de mis recomendados.</p>
<p><!-- /wp:paragraph --></p>
<figure><img width="223" height="229" src="{{ site.baseurl }}/assets/2019/09/debian-logo.png" alt="debian logo" loading="lazy" /></figure>
<h3>Raspbian</h3>
<p>Es el sistema oficial de la Raspberry, una versión adaptada de Debian. Actualmente está en su versión Buster (Debian 10) y es el único sistema <b>oficial</b> para la Raspberry Pi.<br />
Cuenta con una versión Lite, la cual es solo línea de comandos, para servidores. Y Desktop, con un escritorio y programas básicos como navegador, reproductor y <a href="https://www.minecraft.net/es-es/edition/pi/">su propio Minecraft</a>.</p>
<p>			<a href="https://www.raspberrypi.org/downloads/raspbian/" target="_blank" role="button" rel="noopener noreferrer"><br />
						Página oficial de descarga<br />
					</a></p>
<figure><img width="223" height="229" src="{{ site.baseurl }}/assets/2019/09/kali-logo.png" alt="" loading="lazy" /></figure>
<h3>Kali Linux</h3>
<p>Otra distro basada en Debian. Es muy conocida por tener una infinidad de programas de monitorización de redes y hackeo. Tiene una versión ARM apta para Raspberry, la cual puede ser un buen kit de hackeo.<br />
A mi parecer el mejor modelo para usarlo es la Raspberry Pi 4.</p>
<p>			<a href="https://www.offensive-security.com/kali-linux-arm-images/#1493408272250-e17e9049-9ce8" target="_blank" role="button" rel="noopener noreferrer"><br />
						Página oficial de descarga<br />
					</a></p>
<figure><img width="223" height="229" src="{{ site.baseurl }}/assets/2019/09/kodi-logo.png" alt="Kodi Logo" loading="lazy" /></figure>
<h3>Kodi</h3>
<p>Es un centro multimedia de código libre. Se puede usar para añadir contenido a una TV antigua, sirviendo como almacenamiento de películas que guardes. Siempre ha habido mucha controversia por las extensiones de terceros que tiene, las cuales piratean series y canales de televisión.</p>
<p>			<a href="https://libreelec.tv/downloads_new/" target="_blank" role="button" rel="noopener noreferrer"><br />
						Página oficial de descarga<br />
					</a><br />
			<a href="https://www.raspberrypi.org/downloads/raspbian/" target="_blank" role="button" rel="noopener noreferrer"><br />
						Página oficial de descarga<br />
					</a><br />
			<a href="https://www.offensive-security.com/kali-linux-arm-images/#1493408272250-e17e9049-9ce8" target="_blank" role="button" rel="noopener noreferrer"><br />
						Página oficial de descarga<br />
					</a><br />
			<a href="https://libreelec.tv/downloads_new/" target="_blank" role="button" rel="noopener noreferrer"><br />
						Página oficial de descarga<br />
					</a></p>
<figure><img width="268" height="229" src="{{ site.baseurl }}/assets/2019/09/retropie-logo.png" alt="" loading="lazy" /></figure>
<h3>RetroPie</h3>
<p>Muy conocida por los melancólicos, aunque nunca me ha picado la curiosidad de probarlo. Por lo que sé incluye una buena cantidad de emuladores de 8 y 16 bits como SuperNintendo. Se pueden comprar carcasas de consolas y kits que incluyen mandos o una palanca de juegos.</p>
<p>			<a href="https://retropie.org.uk/download/" target="_blank" role="button" rel="noopener noreferrer"><br />
						Página oficial de descarga<br />
					</a></p>
<figure><img width="268" height="229" src="{{ site.baseurl }}/assets/2019/09/nextcloud-logo.png" alt="nextcloud pi" loading="lazy" /></figure>
<h3>Nextcloud Pi</h3>
<p>Es una versión adaptada de Nextcloud a ARM. Es un sistema utilizado como servidor en la nube. Se puede usar dentro de una red LAN o configurar para acceder desde el exterior. Tiene herramientas de visualización de PDF, ofimática en línea, etiquetas, cuotas de disco, envío de correos automáticos... lo que se necesite para crear un servicio en la nube completo.</p>
<p>			<a href="https://ownyourbits.com/nextcloudpi/" target="_blank" role="button" rel="noopener noreferrer"><br />
						Página oficial de descarga<br />
					</a><br />
			<a href="https://retropie.org.uk/download/" target="_blank" role="button" rel="noopener noreferrer"><br />
						Página oficial de descarga<br />
					</a><br />
			<a href="https://ownyourbits.com/nextcloudpi/" target="_blank" role="button" rel="noopener noreferrer"><br />
						Página oficial de descarga<br />
					</a></p>
<h2>Como instalar el sistema operativo</h2>
<p>Para instalar cualquier sistema necesitaremos poner la tarjeta SD en un ordenador y, obviamente, tener descargado el sistema que queramos. Se usará una herramienta para "quemar" el sistema en la SD distinta a la usada para quemar CDs. Personalmente utilizo el programa <a href="https://www.balena.io/etcher/" target="_blank" rel="noopener noreferrer">Balena Etcher</a> en Ubuntu y <a href="https://rufus.ie/" target="_blank" rel="noopener noreferrer">Rufus</a> en Windows, ambos código libre y licencia Apache 2.0 y GPL 3 respectivamente.</p>
<p>Los pasos son sencillos e intuitivos.</p>
<p><!-- wp:image {"id":191,"align":"center"} --></p>
<figure><img src="{{ site.baseurl }}/assets/2019/09/rufus-242x300.png" alt="rufus instalando ubuntu" width="242" height="300" /></p>
<figcaption>Con Rufus solo se necesita indicar el dispositivo donde se quiere flashear el sistema operativo, seleccionar el fichero de este y listo. Es, junto a Etcher, el programa más rápido en hacerlo que he visto.</figcaption>
</figure>
<p><!-- /wp:image --></p>
<p><!-- wp:image {"id":191,"align":"center"} --></p>
<figure><img src="{{ site.baseurl }}/assets/2019/09/capturaetcher.png" alt="pantalla principal de Etcher" width="475" height="300" /></p>
<figcaption>Etcher está disponible para Windows, Linux y Mac. Tiene una interfaz muy agradable y en tres pasos lo hace todo. Las ISOs de Windows suelen dar fallos con este programa.</figcaption>
</figure>
<p><!-- /wp:image -->
<p>Tras instalar el sistema solo se conecta la microSD en su ranura y se conecta todo lo que se necesite, siendo el cable de alimentación lo último en conectar.</p>
<h2>NOOBS nos facilita la vida</h2>
<p><img src="{{ site.baseurl }}/assets/2019/09/noobs-e1569513936850-150x150.png" alt="logo de noobs" width="150" height="150" />A pesar de su nombre (siglas de <b>New Out Of the Box Software</b>, Nuevo Software Listo para Usar en inglés) es una herramienta muy útil y más sencilla de usar que las otras.</p>
<p>Por decirlo de alguna forma es un instalador de sistemas operativos.  Existen dos versiones de NOOBS. La primera permite instalar Raspbian y LibreELEC sin conexión a internet y el resto de sistemas disponibles los puede instalar si se conecta la Raspberry a Internet, la versión Lite necesita conexión para instalar todo.</p>
<p>Al descargar NOOBS no hay que flashear la SD, simplemente arrastras los archivos descomprimidos a la tarjeta y la conectas a la Raspberry. Entonces nos aparecerá un menú donde podremos elegir los sistemas a instalar (sí, pueden ser varios en Dual Boot). Se instalarán solos y elegiremos con cual iniciar por primera vez.</p>
<p>Ya está todo listo para comenzar a usar la Raspberry.</p>
