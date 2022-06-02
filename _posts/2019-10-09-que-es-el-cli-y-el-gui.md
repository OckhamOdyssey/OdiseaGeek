---
layout: post
title: Qué es el CLI y el GUI
date: 2019-10-09 01:40:14.000000000 +02:00
type: post
published: true
password: ''
status: publish
categories:
- Sistemas Operativos Monopuestos y en Red
tags:
- cli
- comandos
- consola
- gui
- instalar
- terminal
- tty
- wayland
- windows
- windows 2016 server
- xorg
---
Recuerdo que la primera vez que escuché CLI y GUI solo podía deducir que podía significar y tuve que buscarlo en Internet. Ambos son conceptos usados frecuentemente en la informática hablando sobre aplicaciones y sistemas operativos.

![](/assets/2019/10/instalacion-windows2016server.png)
<figcaption>Cuadro de instalación de Windows 2016 Server, donde mencionan el GUI como opción instalable.</figcaption>

## Qué es el CLI

Viene del inglés <em>Command Line Interface</em>, significa <strong>interfaz por línea de comandos</strong>. Es el entorno ese feucho de pantalla negra y letras blancas, dónde todo el uso se debe realizar mediante órdenes de comandos y edición de ficheros de configuración. Aquí no se puede utilizar el ratón.

Esta interfaz se encuentra en todos los sistemas operativos y encima de esta se instala una interfaz gráfica (GUI). En Ubuntu por ejemplo la versión CLI es la usada en servidores y sobre estas se instala un entorno de escritorio (Mate, KDE, Cinnamon, Unity...) para las versiones de escritorio.

### Consola TTY

En los sistemas Linux existen diferentes "ventanas" que se pueden utilizar simultáneamente. Estas ventanas van del 1 al 7 y se puede acceder a estos pulsando <code>Ctrl + Alt + Fx</code> siendo la "x" el número deseado. Una de estas TTY, normalmente la TTY1, se usará por el sistema para cargar el entorno gráfico.

Al cambiar de TTY estará en línea de comandos y se podrá usar así o, usando determinados comandos, iniciar el entorno gráfico. También, al no tener nada que ver ni enlazado ningún TTY con otro, aunque inicies sesión con una cuenta en el entorno gráfico puedes usar otro usuario en otro TTY y acceder a este en cualquier momento con la combinación de teclas.

## Qué es el GUI

Son las siglas de <em>Graphical User Interface</em> o <strong>interfaz gráfica de usuario</strong>, esto viene significando que se usa un motor gráfico (Xorg, Wayland) para generar una forma de comunicación con el ordenador con la que se le pueda dar órdenes sin el uso de texto (por decirlo de la forma más coloquial pero correcta que se me ocurre). Vamos, que es el entorno donde se utiliza un ratón, ventanas y esas cosas bonitas.

### CLI emulado dentro del GUI

El GUI tiene aplicaciones como el terminal (distintos nombres según el entorno y la distribución) o el símbolo del sistema (Windows) en las que se interactúa como un CLI, y se podría decir que es como un enlace a este, pero no es más que un emulador, no es un terminal de comandos real, se puede usar el ratón para moverse por el historial o seleccionar secciones de texto.

![Imagen del símbolo del sistema de Windows 7 con la terminal vacía](/assets/2019/10/simbolo-de-sistema-windows7.png)