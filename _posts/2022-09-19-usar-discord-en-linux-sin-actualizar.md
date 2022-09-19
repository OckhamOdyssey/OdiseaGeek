---
title: Usar Discord en Linux sin actualizar
date: 2022-09-19 19:13:12.000000000 +02:00
status: publish
categories:
- Misceláneo
tags:
- discord
- linux
- Arch Linux
- AUR
---
> Esta entrada se ha probado en equipos basados en Debian y en Arch Linux.
{: .prompt-info }

![Ventana de Linux solicitando actualizar Discord](https://i.redd.it/5vlljkiyn3o91.png)

Es bien sabido que la atención que le da Discord a Linux es bastante justita: sin compatibilidad con Wayland, problemas de audio al compartir ventana en XOrg y la falta de repositorios. Aún así, se agradece poder tener un .deb en la página oficial. Nadie sabe el porqué de esta falta de interés, las aplicaciones nativas de Discord están basadas en Electron, por lo que su gestión en todos los sistemas debería ser pan comido.

Para los sistemas Arch o Red Hat, tener un .deb como único modo de instalación es casi como no tener nada. Y digo casi porque, al menos en Arch, tenemos el maravilloso AUR desde el que podemos actualizar Discord, aunque con algo de retraso.

Mientras no tengas Discord actualizado te aparecerá un mensaje como el de la captura de arriba. Existe una forma de evitarlo en caso de no poder descargar o instalar la última versión. Solo tenemos que editar el archivo `~/.config/discord/settings.json` añadiendo la línea ` "SKIP_HOST_UPDATE": true`. El resultado final debería ser algo así:

![Archivo de configuración de Discord señalando la línea añadida.](https://media.discordapp.net/attachments/1020649143330414653/1020649383034900480/unknown.png)

Tras estos cambios podemos volver a abrir Discord sin que nos solicite ninguna actualización.