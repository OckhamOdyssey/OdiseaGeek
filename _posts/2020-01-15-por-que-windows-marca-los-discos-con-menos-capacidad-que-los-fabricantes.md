---
layout: post
title: "¿Por qué Windows marca los discos con menos capacidad que los fabricantes?"
date: 2020-01-15 17:09:08.000000000 +01:00
type: post
parent_id: '0'
published: true
password: ''
status: publish
math: true
categories:
- Sistemas Operativos Monopuestos y en Red
tags:
- binario
- bytes
- decimal
- discos
- linux
- windows
- windows 10
---

Esta entrada va dedicada a un amigo mío que compró recientemente un disco duro de 960GB y se sorprendió al ver que Windows le marcaba mucho menos. Aunque compres un disco de 1TB te marcará aproximadamente 931GB y cuanta más capacidad compres más gigabytes se perderán. Vamos a explicar por qué ocurre esto y quién tiene la culpa. Porque sí, hay un culpable, o varios.

## Unidades de medida en informática

Primero de todo hablemos de como se mide la capacidad de datos en informática. De manera física, toda la información que se transporta por un ordenador funciona con corrientes eléctricas, de forma que todo funciona a base de <strong>hay corriente</strong> y <strong>no hay corriente</strong>, esto se interpreta como 0 y 1. Los datos tanto en los discos duros y los discos de estado sólido son de 0 y 1, cada número de estos se llama <strong>bit</strong> y, la unidad básica de medida se compone de <strong>8 bits y se llama byte</strong>.

A partir de aquí tenemos las unidades más comunes y que se suelen escuchar más a menudo entre la gente. Vamos a ver una tabla con las unidades de medida del sistema internacional.

| Nombre    | Simbolo | Cantidad de bytes |
|-----------|---------|-------------------|
| Kilobyte  | KB      | 10<sup>3</sup>    |
| Megabyte  | MB      | 10<sup>6</sup>    |
| Gigabyte  | GB      | 10<sup>9</sup>    |
| Terabyte  | GB      | 10<sup>12</sup>   |
| Petabyte  | PB      | 10<sup>15</sup>   |
| Exabyte   | EB      | 10<sup>18</sup>   |
| Zettabyte | ZB      | 10<sup>21</sup>   |
| Yottabyte | YB      | 10<sup>24</sup>   |

Estas unidades ascienden exponencialmente en base 10, igual que lo hacen los metros, los gramos. Ahora bien, realmente en informática somos algo especiales. Los metros tienen base 10 porque se usan 10 cifras para generar los números, empezando por el 0 y terminando con el 9, cuando se llega a este número se añade una nueva cifra, las decenas, y así sucesivamente. En informática, como hemos dicho antes, se usa únicamente el 0 y el 1, el código binario, por lo que de forma práctica la unidad de medida tendría que ser en base 2. Esta unidad es la usada y apoyada por el IEEE y vamos a mostrar una tabla con este sistema.

| Nombre    | Simbolo | Cantidad de bytes |
|-----------|---------|-------------------|
| Kibibyte  | KiB      | 2<sup>10</sup>   |
| Mebibyte  | MiB      | 2<sup>20</sup>   |
| Gibibyte  | GiB      | 2<sup>30</sup>   |
| Tebibyte  | GiB      | 2<sup>40</sup>   |
| Pebibyte  | PiB      | 2<sup>50</sup>   |
| Exbibyte  | EiB      | 2<sup>60</sup>   |
| Zebibyte  | ZiB      | 2<sup>70</sup>   |
| Yobibyte  | YiB      | 2<sup>80</sup>   |

De esta forma podemos ver como en lugar de cambiar la unidad cada 1000 bytes lo hacemos cada 2<sup>10</sup>=1024 bytes. Así vemos que 1TiB son 1024GiB, 1 GiB son 1024MiB, 1MiB son 1024KiB... Con esto lo tenemos todo explicado.

## Como juegan los fabricantes con esto

Aquí vamos a ver como y por qué los fabricantes juegan con estas unidades de medida. Primero de todo vamos a usar las tablas de arriba para sacar cuantos bytes son la "misma" unidad de medida. Vamos a utilizar 1 mega en sistema decimal y binario.

$$ 1MB = 1^6B = 1.000.000 bytes $$

$$ 1MiB = 2^{20}B = 1.048.576 bytes $$

Podemos apreciar como 1MiB tiene ligeramente más capacidad que 1MB. También podemos ver como, a medida que aumenta la capacidad, esta diferencia aumenta. Entonces aquí viene el tema de los fabricantes.

Un fabricante, como empresa que es, intenta abaratar gasto, una buena forma que tienen de hacerlo es <strong>fabricar sus componentes usando el sistema decimal y no el binario</strong>, principalmente porque tienen que crear menos capacidad, lo cual es más barato y simple.

El problema viene cuando sabemos que el sistema práctico en la informática es el binario, y anunciar que venden un disco de, por ejemplo, 931GiB no vende tanto que decir que lo venden de 1TB. Ambas afirmaciones son correctas, pero con la última parece que sea más almacenamiento por lo que venden más. Es una <strong>estrategia de marketing</strong> que llevan usando desde siempre.

## Como actúa Windows ante esto

Todos sabemos que Windows es el sistema operativo más usado a nivel de usuario y esto ayuda (o dificulta) a que los usuarios comprendan la informática. En este caso lo empeora ya que <strong>Windows muestra el sistema binario como si fuera decimal</strong>. Esto quiere decir que un disco de 1TB o 931GiB lo marcará como 931GB, lo cual es completamente erróneo. Desconozco completamente el motivo por el que Windows muestra esto así pero hace ver como si los fabricantes fueran unos mentirosos lo cual es mentira, solo cambian el sistema de medida para hacerlo más comercial.

He leído muchos artículos que dicen que las empresas de unidades de almacenamiento nos mienten de esta forma, a demás utilizan el sistema de Windows para demostrarlo, decir que el sistema internacional está en binario. Curiosamente ninguno de los artículos mencionan el sistema de la IEEE ni la diferencia de ambos sistemas.

## Como actúa GNU/Linux ante esto

Ahora viene un punto interesante y olvidado, la posición de Linux ante toda esta absurda batalla de marketing y de tipos de sistemas de medida. Y la respuesta es la indiferencia. Linux no entra en estas batallas y como sistema desarrollado por informáticos competentes cumple todas las normas y muestra las cosas como son. Dependiendo de las herramientas y el sistema que se utilice puede que use el sistema internacional o el de la IEEE siendo este último el más común.

![](/assets/2020/01/disco-linux.png)
_Gestor de particiones de KDE mostrando un disco de 1TB como su capacidad real en binario_
