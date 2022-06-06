---
layout: post
title: Restaurar versión anterior de GRUB en Manjaro
date: 2021-03-18 13:50:49.000000000 +01:00
type: post
parent_id: '0'
published: true
password: ''
status: publish
categories:
- Misceláneo
tags:
- GRUB
- Linux
- Manjaro
---
Hace pocos días tomé la decisión de actualizar el portátil y me sorprendió que el grub estuviese en la lista de actualizaciones. Al poco tiempo, al volver a encender el ordenador, me di cuenta que no aparecía Windows 10 en la lista de arranque. Tras un pequeño e improvisado intento de solucionarlo, el grub dejó de funcionar.

El error que me aparecía era poco común: `symbol_grub_disc_native_sectors not found`

Tras indagar un poco, otro de los errores que me aparecía era: error: `symbol "grub_register_command_lockdown" not found`

Un poco metida de pata, vamos. Al final logré encontrar una lista de mensajes enviados en el registro de bugs de Debian de hace apenas 10 días donde intentaban solucionar este error. En concreto, <a href="https://bugs.debian.org/cgi-bin/bugreport.cgi?bug=984520" target="_blank">estos mensajes</a>.

Las soluciones aportadas eran para Debian, poco me servían. Pero al saber que el problema se trataba de la versión del GRUB decidí bajar a una versión anterior.

## Solución desde live

Era imposible acceder al sistema con el GRUB dañado, así que accedí desde un USB a una versión live de Manjaro. Desde ahí, el sistema ofrece herramientas para acceder al sistema instalado en el ordenador y ejecutar órdenes en este desde el live. Esto es clave para reinstalar el grub.

![](/assets/2021/03/97EnOui.png)

Usando el comando <code>manjaro-chroot -a </code>accedemos al sistema host y todos los cambios que realicemos le afectarán directamente. Ahora tenemos que instalar la herramienta para bajar la versión de los programas.

```terminal
yay -S downgrade
```

![](/assets/2021/03/ZRclBkM.png)

Ahora podemos cambiar el grub a una versión anterior a la 21, que es la dañada. Una vez hecho esto, podemos reiniciar el equipo y ya podremos acceder de nuevo al sistema del ordenador.

##  Mantener la versión antigua del GRUB

Uno de los problemas que tendremos ahora es que el sistema reconocerá que el GRUB está desactualizado e intentará volver a la última versión. Para evitar esto se debe editar la configuración de pacman, el gestor de paquetes de Arch Linux.

```terminal
sudo nano /etc/pacman.conf
```

Añadimos el grub al apartado de paquetes a ignorar para que no se actualice.

![](/assets/2021/03/3lyckGJ.png)

Podemos ver como aparece indicado que se ignorará el paquete grub aunque esté desactualizado. Todo este proceso podemos seguirlo también para otras aplicaciones cuyas últimas versiones son den problemas y volver a actualizarlas cuando se resuelva el problema.

![](/assets/2021/03/UzPaX25.png)