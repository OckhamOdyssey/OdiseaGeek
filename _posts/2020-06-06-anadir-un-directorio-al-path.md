---
layout: post
title: Añadir un directorio al PATH
date: 2020-06-06 18:35:39.000000000 +02:00
type: post
parent_id: '0'
published: true
password: ''
status: publish
categories:
- Apuntes SMR
- Sistemas Operativos Monopuestos y en Red
tags:
- bash
- linux
- PATH
- shell
---
Hace poco un amigo ha tenido problemas con unos comandos que el sistema no le detectaba. El motivo era porque el directorio donde se encontraban no formaba parte del PATH del sistema. El PATH básicamente es una lista con los directorios donde se encuentran los comandos. Al escribir uno en el terminal, el sistema buscará el comando en estos directorios para poder ejecutarlo.

Si el directorio del comando no se encuentra en el PATH habrá que escribir toda la ruta hasta este para ejecutarlo. Saber como añadir un directorio aquí será útil en caso de algún error, de crear nosotros algún comando u otras razones.

## Conocer nuestro shell

Primero necesitaremos conocer el shell que estamos utilizando, cada uno tiene un archivo distinto donde se almacena el PATH. Para conocerlo ejecutamos el siguiente comando:
```terminal
ps -p $$<
```
Este comando nos devolverá el shell que utiliza nuestro sistema, generalmente será bash así que en este post explicaremos cómo añadir un directorio al PATH si se usa bash o zsh, un shell alternativo muy utilizado.

## Acceder al archivo de configuración

Cada intérprete de comandos tendrá un archivo de configuración distinto, este estará oculto dentro de nuestro home. Accederemos mediante comandos usando el editor de texto nano:

Para bash:

```terminal
nano ~/.bashrc
```

Para zsh:

```terminal
nano ~/.zshrc
```

## Añadir directorio

Dentro del archivo en el que estemos añadiremos, sin importar cual de los dos shell sea, la siguiente línea en la parte superior:

`export PATH=directorio:$PATH`

Sustituimos "directorio" por la ubicación de los comandos. Por ejemplo `export PATH=/home/ockham/cmds/bin:$PATH`

Tras guardar y salir del documento tendremos que cerrar sesión y volver a entrar, aunque si sigue dando problemas recomiendo reiniciar.