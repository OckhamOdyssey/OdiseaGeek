---
layout: post
title: Mostrar asteriscos al escribir la contraseña de GNU/Linux
date: 2020-10-17 13:07:00.000000000 +02:00
type: post
parent_id: '0'
published: true
password: ''
status: publish
categories:
- Misceláneo
tags:
- asteriscos
- contraseñas
- gnu
- linux
- seguridad
---

Estoy seguro que, a aquellos que vienen de Windows, el escribir una contraseña en el terminal y que no aparezca nada les resultó chocante, al menos la primera vez. Esto pude ser un problema para usuarios novatos, gente con contraseñas largas o con problemas con el teclado, etc. Para solucionar esto, podemos permitir que aparezcan asteriscos cada vez que escribimos la contraseña en el terminal.

Para ello tenemos que editar el archivo sudoers, este es muy importante por lo que haremos una copia de seguridad por si acaso.

```terminal
sudo cp /etc/sudoers /etc/sudoers.bak
```

Ahora podemos entrar a editar este fichero. <strong>No debe hacerse de la forma tradicional</strong>, usando el comando nano. Usaremos un comando específico para esta operación.

```terminal
sudo visudo
```

Buscaremos la línea que aparece enmarcada y le añadiremos <code><strong>pwfeedback</strong> </code>quedando de la siguiente forma:

![](/assets/2020/10/image-8.png)

Al guardar y salir, podremos comprobar como aparecen los asteriscos al escribir la contraseña.

![](/assets/2020/10/image-9.png)