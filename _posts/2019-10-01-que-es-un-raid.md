---
layout: post
title: Qué es un RAID
date: 2019-10-01 16:37:50.000000000 +02:00
type: post
published: true
password: ''
status: publish
categories:
- Seguridad Informática
tags:
- hardware
- raid
---
Siempre pueden pasar fallos, errores mecánicos a causa del paso del tiempo o algún defecto o accidente. Si esto se produce en un disco duro podríamos llegar a perder toda la información que se guarda en él. Da igual se sea una empresa o un particular, hay veces que vale la pena tener una doble copia de algo, y no me refiero a una copia de seguridad de Windows en caso de haber tocado algo que no se debía, hablo de una copia de un disco enterito. Da igual las copias de seguridad que tengamos, siempre puede haber un pico de tensión eléctrica y fundirnos el disco (aunque para evitar esto están los SAI).

Los <strong>RAID o grupo redundante de discos independientes</strong> es un sistema de almacenamiento de datos que gestiona varios discos para que guarden la información de una forma específica de manera que si se peta uno la información se puede recuperar. Y si te preguntas "¿Y que pasa si se inunda todo o hay un terremoto y se va todo a la mierda?" lo primero de todo buena pregunta, lo segundo: en ese caso tiene que haber un CPD de respaldo a varios kilómetros de distancia.

Ahora bien, existen varios tipos de RAID y varias formas de gestionar los discos, empecemos por esto último.

## Niveles de RAID



### RAID 0

Este RAID no sirve para asegurar la información, pero sigue siendo un RAID. Se encarga de guardar la información en ambos discos como si de uno se tratase. El sistema los interpreta como uno solo y si uno de ellos se estropea la información del segundo estará incompleta y no servirá. Este RAID se utiliza para enviar la información el doble de rápido (en teoría), ya que son dos discos los que están mandando o guardando los datos a la vez, pero no mejora la seguridad, en todo caso la empeora porque al haber dos discos las posibilidades de fallo se duplican.


![](/assets/2019/10/Raid0.png){: w="175" }

### RAID 1

También llamado RAID espejo. Es muy básico y, como su nombre indica, guarda la misma información en ambos discos. Esto hace que si uno deja de funcionar se pueda consultar perfectamente el otro, pero como contra tiene la redundancia y es que al guardarse todo por duplicado la capacidad de un disco no se aprovecha.<br />En estos discos la velocidad de escritura es la misma (en teoría) ya que graban los mismos datos al mismo tiempo pero aumenta la velocidad de lectura, ya que se puede leer el bloque A1 de un disco y el A2 del segundo.

![](/assets/2019/10/Raid1.png){: w="175" }

### RAID 4 y 5

Se que me he saltado muchos pero vamos a los importantes. En el RAID 4 se utiliza como mínimo 3 discos, la información se guarda en dos de ellos como un RAID 0 y el otro se dedica exclusivamente a la paridad. El RAID 5 funciona igual que el RAID 4, su única diferencia es que no hay un disco exclusivo para la paridad, esta se guarda intercalada entre todos los discos.

![](/assets/2019/10/Raid4-1024x636.png){: w="175" }

![](/assets/2019/10/Raid5-1024x636.png){: w="175" }

#### Qué es la paridad

La <strong>paridad </strong>es una forma simple de realizar una recuperación de los datos. Para explicarlo de una forma sencilla y no matar neuronas leyendo el <a href="https://es.wikipedia.org/wiki/Paridad_(telecomunicaciones)" target="_blank">artículo de Wikipedia</a> usaremos la imagen del RAID 4 que tenemos arriba. Haremos que cada bloque sea un número y calcularemos la paridad para el cuarto disco.

![](/assets/2019/10/paridad1.png){: w="175" }

La paridad se calcula con la suma de todos los bloques siendo la paridad de este bloque el resultado de la operación. Para comprobar su efectividad vamos a formatear el disco 2. El sistema, para saber cual es el contenido que corresponde a este disco, tendrá que hacer una ecuación usando la paridad y los otros discos.

![](/assets/2019/10/paridad2.png){: w="175" }

En el caso del RAID 5 funciona exactamente igual.

### RAID 10

![](/assets/2019/10/Raid10-1024x895.png){: w="175" }

Esta es una combinación entre el RAID 1 y el RAID 0 llamado división de espejos. Este RAID necesita un mínimo de 4 discos, dos grupos de dos en RAID 1 y unidos entre ellos en un RAID 0. Esto puede llevar a una implosión cerebral así que mejor ver la imagen para entenderlo.

Estos RAID son buenos para datos de gran valor, porque resiste el fallo de un disco por cada grupo de RAID 1 bajando mucho la posibilidad de pérdida de los datos. A demás es un sistema relativamente rápido porque no se tiene que calcular ninguna paridad.

Existen infinidad de niveles de RAID, estos son los más comunes. El resto de RAIDs suelen estar basados en los aquí mencionados o son muy enrevesados o ineficaces. Dependiendo también de las características y lo que se necesite se escogerá uno u otro, cada situación es distinta.

## Tipos de RAID

Hay distintas formas de conectar un RAID, cada uno con unas características distintas. Algunas son para servidores en rack pero vamos a hablar de tres que pueden usarse en pymes y particulares.

### Por hardware

Se conecta al ordenador una extensión dedicada. El sistema operativo detecta solo un disco y lo trata como tal y es esta extensión la encargada de gestionar y dividir la información del RAID entre los dos discos.

![](/assets/2019/10/raidhardware-300x225.png)
<figcaption>Esta extensión PCIe tiene dos puertos SATA para conectar los dos discos aquí en lugar de a la placa base.</figcaption>

### Por software

Este es un RAID un poco pocho. El ordenador no detecta ni gestiona el RAID, de esto se encarga el sistema operativo tras la instalación. En los sistemas Windows se puede entrar en el administrador de discos y en un par de clics tenerlo funcionando. Recuerdo que en Linux se podía configurar con el comando <strong><code>mdadm</code></strong>. El problema es que depende del SO, gasta recursos de este y como cambies el sistema o sea este el que falle tendrás dos discos algo inútiles y será más tedioso arreglarlo.

### RAID falso

Se llama así porque simula ser un RAID por hardware siendo realmente por software. El firmware que lo gestiona se encuentra incorporado en la placa base del ordenador, lo que significa que no nos tenemos que preocupar por los posibles problemas con el sistema operativo y no gasta recursos del sistema, pero sí de la placa, cosa que se podría aprovechar para otra cosa (que se note que mi favorito es el de hardware). Pero que no os confunda el nombre, sigue siendo mejor que el que funciona por software.