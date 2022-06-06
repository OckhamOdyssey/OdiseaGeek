---
layout: post
title: El arranque del sistema, medidas de seguridad y el GRUB.
date: 2020-04-10 16:28:51.000000000 +02:00
type: post
parent_id: '0'
published: true
password: ''
status: publish
categories:
- Apuntes SMR
- Seguridad Informática
- Sistemas Operativos Monopuestos y en Red
tags:
- arranque
- grub
- linux
- windows
---

Seguro que todos los que estamos leyendo este post hemos tenido uno de esos ordenadores que al encenderlo soltaba un pitidito, o más de uno. Mi maravilloso ordenador de torre con Windows Vista siempre soltaba dos al encenderse, y en aquella época yo era tan curioso que nunca me pregunté qué significaban, era feliz mientras el ordenador arrancase.

Vamos a ver no solo qué significan esos pitidos sino todo el sistema que lo ejecuta y todo lo que ocurre entre que pulsas el botón de encender hasta que ves como arranca el sistema operativo.

## BIOS

El <strong>sistema básico de entrada y salida</strong>, BIOS por sus siglas en inglés, es un tipo de firmware incluido en todas las placas base el cual se pone en funcionamiento cuando uno inicia el ordenador. Usualmente también se conoce a la BIOS como <strong>Legacy</strong>, debido a que es un sistema antiguo y la UEFI lo ha sustituido casi completamente. La BIOS se encarga de detectar los componentes del ordenador, asegurarse que funcionan correctamente y buscar en los dispositivos de almacenamiento un gestor de arranque para darle todo el control del hardware al sistema operativo.

Cuando encendemos el ordenador tenemos un pequeño periodo de tiempo en el que podemos acceder a la BIOS pulsando unas teclas concretas, estas dependen del fabricante de la placa base. Dentro de la BIOS tenemos varias posibles configuraciones, destacaremos algunas en este post, como por ejemplo el poder elegir la prioridad de dispositivos a la hora de buscar un sistema de arranque o la posibilidad de cambiar entre BIOS y UEFI.

### UEFI

La <strong>interfaz de firmware extensible unificada</strong>, UEFI por sus siglas en inglés, apareció hace unos años para jubilar a la BIOS tras más de 45 años de leal servicio. La UEFI hace exactamente lo mismo que la BIOS, con ciertas mejoras y añadidos:

<ul>
<li>Gestión más eficiente de la energía.</li>
<li>Iniciar de forma más rápida.</li>
<li>Admite particiones de más de 2TB.</li>
<li>La posibilidad de gestionar los dispositivos PCIe.</li>
<li>Gestión de fallos mejorada.</li>
<li>Mejor interfaz gráfica, pudiendo usar el ratón.</li>
<li>Actualizar su firmware desde Internet.</li>
<li>Incluye Secure Boot, el cual se explicará más adelante.</li>
</ul>

## Qué es el POST

El POST es conocido como auto-prueba de arranque, forma parte de las medidas de seguridad de la BIOS y se encarga de realizar pruebas a los dispositivos antes de ejecutar la BIOS para asegurarse que ninguno está dañado a nivel de software ni de hardware. Técnicamente, la placa base lanzará un pitido de confirmación cuando el POST haya terminado de realizar las pruebas pero en los ordenadores más modernos me estoy encontrando placas que no sueltan ningún pitido de confirmación, solo cuando hay fallos.

Los pitidos también suenan para identificar fallos, pueden variar según el fabricante pero en general suelen significar lo mismo. Durante mi estancia en prácticas en el departamento de Devoluciones y Garantías lidiaba diariamente con componentes dañados, por lo que los pitidos de error del POST eran comunes, a continuación añado algunos de los pitidos de error más comunes y sus significados:

| **Tonos**               | **Significado**                      |
|-------------------------|--------------------------------------|
| Tono ininterrumpido     | Fallo en el sistema de alimentación. |
| 1 tono largo            | No detecta memoria RAM.              |
| 1 tono largo y 2 cortos | Fallo en la tarjeta gráfica          |
| 3 tonos cortos          | RAM defectuosa.                      |
| 5 tonos cortos          | Fallo en el procesador               |
| 10 tonos cortos         | Error en el CMOS.                    |

## Secure Boot

El Secure Boot es un sistema de seguridad de Microsoft para las BIOS que, como se puede deducir, favorece a los sistemas Windows. Su intención es evitar los Bootkits, un tipo de malware que toma el control del ordenador haciéndose pasar por un sistema operativo, bloqueando todo sistema operativo que no sea de "confianza". De nuevo, como se puede deducir, los sistemas de "confianza" de Microsoft son sus propios sistemas operativos algún otro, pero se pueden contar con los dedos de una sola mano.

Si se usa un sistema Windows es una buena idea mantener esta opción encendida, ya que da un plus de seguridad, pero si se va a instalar un GNU/Linux o un Dual Boot conviene desactivarlo para evitar problemas.

## Los gestores de arranque y el GRUB

Los gestores de arranque sirven para que la BIOS detecte dónde se encuentran almacenados los sistemas operativos y pueda darles el control al terminar todos los procesos que se indican arriba. Se instalan junto al sistema operativo y sirve de intermediarios entre estos y las BIOS.

![](/assets/2020/04/Screenshot_1-1024x457.png)

Existen gestores de arranque para varios sistemas operativos. Windows tiene el suyo propio y puede hacer que se pueda elegir entre distintos Windows antes de arrancarlos, pero a penas soporta otros sistemas. Yo todavía no he visto un sistema Linux que sea soportado por este sistema.

Por otro lado está el llamado <strong>GRUB</strong>, el gestor de arranque múltiple. Este gestor de arranque permite elegir entre distintos sistemas, instalados en distintos discos, sin importar cual sea. Desde versiones Windows, distribuciones basadas en Ubuntu, CentOS... lo que sea.