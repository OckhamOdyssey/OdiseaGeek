---
layout: post
title: Actualizar la distribución de Ubuntu sin borrar datos
date: 2020-02-11 16:41:33.000000000 +01:00
type: post
parent_id: '0'
published: true
password: ''
status: publish
categories:
- Misceláneo
- Sistemas Operativos Monopuestos y en Red
tags:
- datos
- distribución
- distro
- Kubuntu
- ubuntu
- versión
---
Al igual que los sistemas Windows, las distribuciones de Ubuntu también tienen su periodo de vida útil. Cuando pasa este tiempo las versiones dejan de recibir soporte y actualizaciones de seguridad por lo que se vuelven vulnerables a futuros ataques. Vamos a hablar no solo de cómo actualizar a la última versión de Ubuntu sino qué versión elegir.

## Qué versión de Ubuntu escoger

Ubuntu y todas sus variantes (así como Kubuntu o Lubuntu entre otras) tienen dos tipos de versiones. Las versiones estándar tienen una duración de 9 meses y las versiones extendidas tienen un soporte de 5 años.

Por motivos obvios vamos a elegir las versiones de larga duración, estas son llamadas [LTS]({% post_url 2020-03-13-como-funcionan-las-versiones-de-ubuntu-que-son-las-versiones-lts %}) y lanzan una cada dos años. En el momento de escribir este blog la última versión extendida es la de Ubuntu 18.04 LTS con soporte hasta 2023 y dentro de dos meses lanzarán la versión 20.04 LTS y tendrá soporte hasta 2025.
Existen motivos para utilizar las versiones de corta duración, pondré mi caso como ejemplo. Mi ordenador portátil traía por defecto Kubuntu 18.04.2 LTS pero al cabo de varios meses quería personalizar profundamente su apariencia, para lo cual necesitaba una versión del framework Qt que mi sistema no tenía. La forma más fácil de solucionar esto era actualizar a Kubuntu 18.10, cuyo soporte terminó en julio de 2019 y yo, como buen incauto, aún lo tenía instalado.

Siempre hay que fijarse cuando termina el soporte del sistema que vas a instalar, por ejemplo, el soporte de la versión 19.04 terminó el mes pasado por lo que hay que pasar a la siguiente, la 19.10. Obviamente también hay que mirar qué novedades ofrecen estas versiones, si son interesantes o compatibles con lo que se tiene instalado.

## Actualizar la versión de Ubuntu

Vamos a buscar la aplicación llamada "Software Y Actualizaciones", entraremos en la tercera pestaña estableceremos el "notificarme de una nueva versión de Ubuntu" del menú desplegable "Para cualquier nueva versión".

Tras esto abrimos el terminal y ejecutamos como súperusuario los siguientes comandos.

```terminal
sudo apt update
```

Es muy probable que esta herramienta ya esté descargada, ahora comprobaremos que instalaremos la última versión, aunque no sea de larga duración. Para ello escribimos el siguiente comando.

```terminal
sudo nano /etc/update-manager/release-upgrades
```

Esto nos llevará a un archivo de configuración, en la última línea tenemos que comprobar que pone `Prompt=normal`, de no ser así lo cambiaríamos.

Ahora toca cambiar todos los repositorios a las versiones que queramos. Cada versión de Ubuntu tiene un nombre, a continuación pondré los de las últimas versiones. 

| Version de Ubuntu | Nombre de version |
|-------------------|-------------------|
| 16.04 LTS         | Xenial            |
| 16.10             | Yakkety           |
| 17.04             | Zesty             |
| 17.10             | Artful            |
| 18.04 LTS         | Bionic            |
| 18.10             | Cosmic            |
| 19.04             | Disco             |
| 19.10             | Eoan              |

Ahora escribiremos el siguiente comando sustituyendo "<strong>antiguo</strong>" por la versión actual (`bionic` en mi caso) y "<strong>nuevo</strong>" por la versión nueva (`eoan` en mi caso). <strong>Importante sin mayúsculas</strong>.

```terminal
sudo sed -i 's/antiguo/nuevo/g' /etc/apt/sources.list
```

Ahora actualizaremos los repositorios para tener los adecuados. Como tiene que descargar todos los paquetes nuevos es probable que tarde bastante.

```terminal
sudo apt update && sudo apt upgrade -s
```

Una vez ha terminado ejecutamos el comando de actualización de distribución.

```terminal
sudo apt dist-upgrade
```

## Tras la actualización

Es posible que el ordenador se reinicie alguna vez durante la actualización, es normal. Al finalizar ejecutaremos un comando para eliminar los paquetes obsoletos.

```terminal
sudo apt autoremove && sudo apt clean
```

Si tenemos algún problema leeremos en la respuesta de la terminal qué ocurre, es posible que se pueda solucionar ejecutando el siguiente comando.

```terminal
sudo apt --fix-broken install
```

Una vez actualizados todos los paquetes y reiniciado el ordenador podremos ver que ya tenemos la nueva distribución instalada en nuestro ordenador sin haber perdido las aplicaciones ni los archivos.
