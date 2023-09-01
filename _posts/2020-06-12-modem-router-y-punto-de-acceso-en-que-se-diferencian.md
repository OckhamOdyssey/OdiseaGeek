---
layout: post
title: Módem, router y punto de acceso. En qué se diferencian.
date: 2020-06-12 22:01:50.000000000 +02:00
type: post
parent_id: '0'
published: true
password: ''
status: publish
categories:
- Apuntes SMR
- Servicios en Red
tags:
- dns
- módem
- punto de acceso
- raspberry
- router
---

Es posible que muchos no conozcan el nombre real o técnico del aparatejo que les da Internet a sus hogares. Aunque suele ser conocido comúnmente como "router", puede llegar el caso en el que tengamos que diferenciar un router de lo que realmente estamos usando. Es por ello que siempre va a venir bien conocer los distintos tipos de aparatos que nos podemos encontrar.

## Módem

Puede que identifiquemos un módem como ese cacharro antiguo que se conectaba a la línea telefónica, hacía un ruido espantoso, y nos daba conexión a Internet siempre y cuando no descolgásemos el teléfono.

Pues en parte sí, eso es un módem, pero hay otras cosas que lo son. Su definición es la del dispositivo encargado de la conversión de la señal a modo de que un ordenador pueda interpretarla. En resumidas cuentas <strong>es un conversor</strong>, y el <strong>encargado de ponernos en contacto con nuestro proveedor de Internet</strong>, ya sea mediante fibra óptica, cable coaxial o el anticuado ADSL.

![](/assets/2020/06/modem-adsl.png)
_Módem ADSL. Se puede reconocer el conector telefónico y el Ethernet._

## Router

El más conocido por la gente. Un router es el <strong>encargado de crear una red funcional </strong>y comunicar todos los dispositivos a esta. Un router, si se configura debidamente, también es capaz de comunicar dos redes locales entre sí.

El router no solo incluye servicios, así como el [DNS]({% post_url 2019-09-30-que-es-el-ddns %}) o el DHCP, también tiene la <strong>función NAT</strong>, conocida también como enmascaramiento de IP o traducción de direcciones. Esta función permite que nuestros dispositivos se conecten con la red pública (Internet por ejemplo) utilizando el router como intermediario, actuando como capa para que no se conozca la estructura de nuestra red local. También permite que se comparta una sola dirección IP pública para todos los dispositivos de la red local, ahorrando una cantidad considerable de IP's, las cuales son un gasto de dinero elevado.

Los routers también suelen tener incorporados varios puertos Ethernet, cumpliendo también la función de switch.

## Punto de acceso

Los puntos de acceso inalámbricos (<em>Wireless Access Point</em>) son dispositivos que crean una red inalámbrica (<em>Wi-Fi</em>) para que los dispositivos se puedan conectar remotamente. Estos puntos gestionan también la encriptación inalámbrica a base de un nombre para la red (UUID) y una contraseña. El sistema actual de encriptación suele ser el WPA2.

## 3 en 1

Seguro que mientras leías todo este embrollo pensarías "el cacharro que tengo en mi casa hace todo eso, entonces, ¿cuál de todas las cosas es?". La respuesta es simple, todas.

Para que una red local al uso funcione a la perfección requiere de estos tres complementos. Un particular no va a comprar estas tres cosas para tener WiFi en casa, es por ello que las compañías suelen ofrecer (incluido en el contrato) un dispositivo 3 en 1. Este creará una red WiFi en el hogar para que se conecten todos los dispositivos usando el punto de acceso, gestionará la red y les pondrá en contacto con el exterior mediante con el router y le dará acceso a Internet con el módem.

Estos dispositivos 3 en 1 suelen ser los que llamamos directamente "routers" y el que la gran mayoría de personas tienen en sus casas. Dependiendo de la compañía que se tenga contratada, será más laxa o menos a la hora de configurar tú mismo la red, añadiendo tu propio router para mayores gestiones o tu propio punto de acceso para ampliar la señal wifi.

![](/assets/2020/06/modem.jpg)
_De izquierda a derecha podemos apreciar dos conectores coaxiales (módem), cuatro puertos Ethernet (router/switch) y una antena para la conexión innalámbrica (punto de acceso)._

![](/assets/2020/06/conexion-router-modem-raspberry-768x1024.jpg)
_En esta imagen podemos ver el 3 en 1 de mi operadora de internet, modificado para que solo actúe de módem. Mi router TP-Link que actua también como punto de acceso. Abajo mi Raspberry con un disco duro de 1TB como servidor en la nube._