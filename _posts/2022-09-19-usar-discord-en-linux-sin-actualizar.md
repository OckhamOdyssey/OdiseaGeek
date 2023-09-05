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
> Esta entrada se ha probado en equipos basados en Debian y en Arch Linux a día 19/09/2022 y 05/09/2023 respectivamente.
{: .prompt-info }

![Ventana de Linux solicitando actualizar Discord](https://i.redd.it/5vlljkiyn3o91.png)

Es bien sabido que la atención que le da Discord a Linux es bastante justita: sin compatibilidad con Wayland, problemas de audio al compartir ventana en XOrg y la falta de repositorios. Aún así, se agradece poder tener un .deb en la página oficial. Nadie sabe el porqué de esta falta de interés, las aplicaciones nativas de Discord están basadas en Electron, por lo que su gestión en todos los sistemas debería ser pan comido.

Para los sistemas Arch o Red Hat, tener un .deb como único modo de instalación es casi como no tener nada. Y digo casi porque, al menos en Arch, tenemos el maravilloso AUR desde el que podemos actualizar Discord, aunque con algo de retraso.

Mientras no tengas Discord actualizado te aparecerá un mensaje como el de la captura de arriba. Existe una forma de evitarlo en caso de no poder descargar o instalar la última versión. Solo tenemos que editar el archivo `~/.config/discord/settings.json` añadiendo la línea `"SKIP_HOST_UPDATE": true` **al final del JSON**, muy importante esto. Un ejemplo del resultado:

```json
{ 
  "IS_MAXIMIZED": true,
  "IS_MINIMIZED": false,
  "WINDOW_BOUNDS": {
    "x": 2240,
    "y": 219,
    "width": 1280,
    "height": 720
  },
  "SKIP_HOST_UPDATE": true
}
```

Tras estos cambios podemos volver a abrir Discord sin que nos solicite ninguna actualización.