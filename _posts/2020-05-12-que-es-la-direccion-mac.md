---
layout: post
title: Qué es la dirección MAC
date: 2020-05-12 13:33:00.000000000 +02:00
type: post
parent_id: '0'
published: true
password: ''
status: publish
categories:
- Apuntes SMR
- Servicios en Red
- Sistemas Operativos Monopuestos y en Red
tags:
- Mac
---
Es probable que muchos de hayamos oído hablar de las direcciones IP, incluso que sepamos para qué sirven, pero las direcciones MAC son algo más desconocidas aunque igual de necesarias. En esta entrada no solo veremos qué es la dirección MAC sino también alguno de sus usos.

## Qué es

Las siglas de la <strong>dirección MAC</strong> vienen de <em>Media Access Control</em>, aunque en español también se la conoce como <strong>dirección física</strong>, y su función básica radica en identificar una tarjeta de red de forma global. Para los que no hayan entendido nada básicamente es el número de pasaporte de la tarjeta de red de nuestro ordenador, un número único en todo el mundo.

Este número es único y exclusivo para identificar tarjetas de red, puesto que son las encargadas de enviar y recibir información del exterior. Esto incluye las tarjetas de red integradas en las placas base (que suelen ser Ethernet), tarjetas Wi-Fi del ordenador o del móvil, tarjetas Bluetooth.

En teoría, al ser parte de un componente físico, la dirección MAC no puede ser modificada. La realidad es muy distinta, Internet está repleto de tutoriales que muestran cómo hacerlo de una forma muy simple, incluso algunos dispositivos vienen con herramientas para cambiar la MAC. La finalidad de cambiarla siempre es ocultar un identificador del dispositivo, ya sea por privacidad o con malas intenciones.

### Funcionamiento

Es un número hexadecimal de 48 bits separados en 6 bloques de 8 bits. Los primeros 3 bloques identifican al fabricante de la tarjeta, los 3 últimos es un número identificativo gestionado por la IEEE.

Como una imagen vale más que mil palabras, vamos a insertar una imagen para explicar esto mejor.

![](/assets/2020/05/MAC-1024x509.png)

Esta estructura evita un descontrol al estar regularizado por la IEEE y permite identificar fácilmente algunos dispositivos por el identificador del fabricante.

### Diferencias entre dirección IP y dirección MAC

Ambas son direcciones de identificación a la hora de comunicarse con otros ordenadores. La diferencia principal es que la dirección IP puede variar mientras que la dirección MAC va a ser siempre la misma, esto nos da una serie de ventajas que veremos en otros apartados.

<ul>
<li>Las direcciones IP tienen 4 grupos de dígitos decimales mientras que las direcciones físicas constan de 6 grupos de dígitos hexadecimales.</li>
<li>Las direcciones MAC se pueden identificar con dos puntos o un guión. Las IP siempre van a separarse por un solo punto, solo incluirán dos puntos para indicar el puerto. </li>
<li>Las direcciones IP pueden variar por distintas razones, una dirección MAC es permanente.</li>
</ul>

## Su uso en las redes locales

Puede que un usuario al uso no le de mucha importancia pero una buena organización dentro de la red local (sea de empresa o la simple red Wi-Fi de un particular) es importante, tiene distintos beneficios como una mejora de seguridad.

Dentro de mi propia red he usado la dirección MAC de algunos dispositivos para indicarle al router que les mantenga siempre la misma dirección IP. Esto me sirve para hacer instalaciones más fácilmente, por ejemplo, mantener la misma IP a una impresora hará que los ordenadores y móviles la detecten más fácilmente y evita que se desconfigure o duplique. También mantengo una IP estática en mi Raspberry con Nextcloud, para que el propio router pueda hacer una redirección de puertos y se pueda acceder desde fuera mediante un [DDNS]({% post_url 2019-09-30-que-es-el-ddns %}).

![](/assets/2020/05/direcciones-mac-router.png)
_El identificador de producto de las MAC y la IP de Nextcloud están censuradas por seguridad.<br />El nombre del propietario del iPhone por razones obvias._

La tabla de arriba corresponde a mi propio router, se puede observar como se ha asignado direcciones IP permanentes dependiendo de la MAC. En el momento de la captura mi impresora y otra Raspberry se encontraban apagadas, por ello no se puede ver cómo la impresora también tiene IP estática o como el identificador del fabricante de las dos Raspberrys coincide.

Con respecto a los identificadores, se puede ver como el iPhone y el iPad no tienen el mismo ID de fabricante, esto se debe a que también se otorgan distintos identificadores al mismo fabricante. Todos los identificadores se pueden encontrar a través de la <a href="https://hwaddress.com/company/apple-inc/" target="_blank">página de direcciones físicas</a>.

## Su uso en los videojuegos

No es algo que suela verse a menudo pero las direcciones MAC también son usadas en los videojuegos en la lucha constante contra las trampas online. Es por eso que muchas compañías, a la hora de bannear a un jugador, pueden tomarlas siguientes consideraciones:

<ul>
<li>Bloquear su cuenta puede ser innefectivo al poder crearse más de una.</li>
<li>Restringir al jugador en base a su dirección IP es todavía más innefectivo e injusto, ya que una IP puede cambiar con el tiempo y podría bloquear a un jugador inocente. Dependiendo del tipo de red también podrían verse afectados varios jugadores.</li>
<li>El bloqueo mediante la MAC no perjudica a otros usuarios pero es fácilmente evasivo con un clonado de MAC o simplemente comprando una tarjeta de red nueva. También generaría un problema en el supuesto caso de que el hacker vendiese su placa a otra persona, esta segunda heredaría el bloqueo.</li>
</ul>

Por ello usualmente las compañías se mantienen aferradas al primer método, especialmente en juegos de pago. Al tener que crear otra cuenta, deben comprar de nuevo el juego, generando así ingresos extra a la compañía.

Matizo que en ningún momento he indicado que el bloqueo mediante MAC sea el más común o efectivo, solo es una herramienta más que, si mal no recuerdo, ha causado polémica e compañías de videojuegos como Valve.