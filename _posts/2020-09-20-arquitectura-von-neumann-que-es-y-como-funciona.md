---
layout: post
title: Arquitectura Von Neumann. Qué es y cómo funciona
date: 2020-09-20 15:53:37.000000000 +02:00
type: post
parent_id: '0'
published: true
password: ''
status: publish
categories:
- Apuntes ASIR
- Apuntes SMR
- Fundamentos del Hardware
- Implantación de Sistemas Operativos
- Sistemas Operativos Monopuestos y en Red
---
>Puedes profundizar conceptos en la segunda parte; con más datos, detalles e imágenes. [Arquitectura Von Neumann. Qué es y cómo funciona]({% post_url 2020-03-13-como-funcionan-las-versiones-de-ubuntu-que-son-las-versiones-lts %}).

Los primeros ordenadores, surgidos en la década de los 40 en adelante, no eran más que armatostes de habitaciones enteras que funcionaban a base de válvulas y tubos de vacío. Estas máquinas estaban <strong>construidas y no programadas </strong>para cumplir una función específica, esto significaba que cada vez que tenía que cambiar de tarea se requerían semanas para diseñar y cambiar toda su estructura.

![](/assets/2020/09/1280px-Eniac.jpg)
_Imagen del <a href="https://es.wikipedia.org/wiki/ENIAC" target="_blank" rel="noreferrer noopener">ENIAC</a>, uno de los primeros ordenadores - <a href="http://ftp.arl.mil/ftp/historic-computers" target="_blank" rel="noreferrer noopener nofollow">U.S. Army Photo</a>, Dominio público, <a href="https://commons.wikimedia.org/w/index.php?curid=55124" target="_blank" rel="noreferrer noopener nofollow">Enlace</a>_

En respuesta a esto el científico John Von Neumann ideó una arquitectura con una unidad de procesamiento central y una memoria. Esta última era la encargada de dar las instrucciones al procesador sobre cómo tenía que comportarse. El procesador mantiene una estructura modular, solo<strong> hay que reprogramarlo, no reconstruirlo.</strong>

## Componentes de la estructura Von Neumann

![](/assets/2020/09/arquitectura-von-neumann-pocha.png)

<ul>
<li><strong>CPU</strong>, siglas de Unidad de Procesamiento Central en inglés. Dirige el funcionamiento del ordenador y es el encargado de realizar las operaciones necesarias para obtener los datos que se solicitan.
<ul>
<li>La Unidad Aritmético-Lógica, o <strong>ALU</strong>,  realiza las operaciones de cálculo del ordenador. Por ejemplo las sumas, operaciones AND y OR, o comparaciones de valores.</li>
<li>La <strong>UC</strong>, o Unidad de Control, hace de intermediaria entre la ALU y la memoria principal. Obtiene la operación de la memoria y los datos necesarios para ejecutarla, al finalizar devuelve el resultado a la memoria.</li>
<li>Los <strong>registros</strong> almacenan temporalmente la información del proceso que se está realizando. Un ejemplo de registro es el "<em>registro acumulador</em>", este guarda los resultados temporales de una operación que se encuentra en curso en la ALU.</li>
</ul>
</li>
<li>La <strong>memoria principal</strong> almacena las instrucciones que debe ejecutar el procesador y los datos necesarios para realizarlas. Aunque es poco común escuchar este nombre, es otra forma de llamar a la <strong>memoria RAM</strong> de los ordenadores modernos.</li>
<li>Los <strong>dispositivos de E/S</strong>, o entrada y salida, permiten la comunicación del ordenador con el exterior, usualmente con el usuario.</li>
<li>Los <strong>buses</strong> (marcados como flechas en la ilustración) son  los canales de comunicación entre los componentes del ordenador.</li>
</ul>

## Ciclo de instrucción

Teniendo en cuenta lo lioso que puede resultar el esquema (que parece un cajero automático) y las descripciones, vamos a explicar paso a paso, y de forma muy simple, cómo trabaja un procesador desde el momento en el que se inicia hasta el apagado.

<ol>
<li>Leer la instrucción:
<ul>
<li>La UC detecta cual es la instrucción a realizar y se la solicita a la memoria principal.</li>
<li>La memoria principal transfiere la instrucción al registro.</li>
<li>Si existe algún dato necesario para realizar la instrucción, también se envía al registro.</li>
</ul>
</li>
<li>Ejecutar la instrucción:
<ul>
<li>La Unidad de Control envía la instrucción y los datos necesarios a la ALU.</li>
<li>La ALU ejecuta la instrucción y obtiene el resultado.</li>
<li>La UC envía el resultado a la memoria principal y lee cual es la siguiente instrucción, iniciando el ciclo de nuevo.</li>
</ul>
</li>
</ol>