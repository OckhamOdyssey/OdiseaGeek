---
layout: post
title: Cómo y porqué activar la licencia de Windows en máquinas virtuales
date: 2021-05-09 20:47:22.000000000 +02:00
type: post
parent_id: '0'
published: true
password: ''
status: publish
categories:
- Apuntes SMR
- Sin categoría
- Sistemas Operativos Monopuestos y en Red
tags:
- licencia
- virtual box
- windows
- windows 10
- windows 2016 server
- windows 7
---

> Esta no es una solución oficial, puede no ser válida según la máquina virtual, modelo o versión del sistema operativo.
{: .prompt-warning}

Todos sabemos que para utilizar todas las funciones de los sistemas Windows se necesita introducir una licencia. Las licencias son códigos que demuestran que has pagado por el producto y debes introducir en el sistema para tener todas las funcionalidades. Pagar por usar una máquina virtual tiene poco sentido, Microsoft lo sabe, por ese mismo motivo sus máquinas virtuales pueden activarse sin poseer una licencia.

## Por qué activar Windows en una máquina virtual

Microsoft permite el uso de Windows sin licencia por tiempo ilimitado, pero restringe ciertas funciones que pueden ser importantes. Cambiar le tema o el fondo de pantalla estará bloqueado si Windows no está activado. A parte que aparecerá una marca de agua en la parte inferior derecha. Si se trata de una máquina Windows Server las restricciones también son mayores, ya que <strong>la máquina se reiniciará</strong> de forma aleatoria, algo bastante grave en servidores.

Aunque no está 100% probado, siempre he experimentado pérdidas de rendimiento en las máquinas que no se solucionaba hasta activar Windows.

## Cómo activar Windows gratis en máquinas virtuales

Este proceso funciona en máquinas Windows 10, Windows 7 y las distintas versiones de Windows Server.

Lo primero de todo es comprobar que Windows no está activado, en muchas ocasiones se activa solo al detectar que se encuentra en una máquina virtual. Para ello debes acceder al apartado de sistema de la máquina virtual y revisar el apartado de activación.

![](/assets/2021/05/6e8QLqe.png)
_Muestra de activación en Windows 7_

En caso de que no esté activado debemos abrir un terminal como administrador. Podemos comprobar que tenemos los privilegios porque aparecerá "system32" en en prompt.

![](/assets/2021/05/3A9Mmtk.png)

Tras esto escribimos el siguiente comando:

```terminal
slmgr /rearm
```

Nos aparecerá la siguiente ventana, la cual nos indica que se ha renovado la licencia de Windows en la máquina virtual.

![](/assets/2021/05/image.png)

Tras esto, reiniciaremos la máquina virtual y deberíamos contar con una licencia válida.

## Un apunte más...

Me gustaría aclarar que este proceso solo funciona en máquinas virtuales, para poder usar Windows en una máquina real se debe comprar una licencia legal. Desde este blog no fomentamos en ningún momento el uso de piratería ni daremos facilidades para su uso.