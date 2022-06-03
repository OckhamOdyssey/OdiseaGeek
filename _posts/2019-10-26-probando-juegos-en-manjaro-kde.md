---
layout: post
title: Probando juegos en Manjaro KDE
date: 2019-10-26 12:36:42.000000000 +02:00
type: post
parent_id: '0'
published: true
password: ''
status: publish
categories:
- Misceláneo
tags:
- Arch Linux
- drivers
- juegos
- KDE
- Kubuntu
- linux
- Manjaro
- Minecraft
- Plasma
- Proton
- ProtonDB
- shooter
- Steam
- Valve
- windows
---
>ACTUALIZACIÓN: Se ha publicado una entrada más reciente con fecha a 5/3/22. [Pulse aquí para visitarla]({% post_url 2022-03-05-mi-experiencia-en-el-mundo-gaming-con-linux-por-que-el-futuro-se-ve-prometedor %}).
{: .prompt-info }

En la sección [Sobre mí](https://odiseageek.es/about/) he comentado los ordenadores que tengo ahora mismo, entre estos se encuentra un ordenador de torre montado a piezas con Windows 10. Desde hace un tiempo he querido cambiarme el sistema operativo a uno Linux y me he decantado por instalar Manjaro KDE.

## ¿Por qué Manjaro?

![](/assets/2019/10/manjaro-1024x1024.png){: width="972" height="589" style="max-width: 200px" .left}

Tenía decidido que quería instalarme un sistema operativo con KDE Plasma, mi primera opción durante mucho tiempo ha sido <a href="https://kubuntu.org" target="_blank">Kubuntu</a>, el mismo sistema que tengo en mi ordenador portátil, pero desde hace unos días (en concreto desde la liberación de <a href="https://kde.org/announcements/plasma-5.17.0.php?site_locale=es" target="_blank">Plasma 5.17</a>) me ha entrado el gusanillo de probar un sistema rolling release, en concreto Manjaro. Si no lo conoces, <a href="https://manjaro.org" target="_blank">Manjaro</a> es un sistema operativo GNU/Linux basado en <a href="https://www.archlinux.org" target="_blank">Arch Linux</a>. Con este último sistema he tenido mis pequeñas peleas, me ha dado problemas de instalación en otras ocasiones así que decidí por su derivado, el cual he escuchado que es más "friendly".</p>
>ATUALIZACIÓN: El 8/7/21 documenté una instalación de ArchLinux con encriptación. [Pulse aquí para visitarla]({% post_url 2021-07-08-instalar-archlinux-con-btrfs-y-encriptacion-luks %}).
{: .prompt-info }

## Preparándolo para jugar

La gracia de mi ordenador de torre es poder usarlo para jugar y viendo los problemas de drivers gráficos que tuve durante mi primera instalación de Kubuntu, en un portátil Acer Aspire E5, no tenía ganas de pasar por lo mismo. Todos sabemos la controversia que hay entre Linux y los videojuegos. Steam ha ayudado mucho a que los juegos se puedan jugar en Linux y le estamos muy agradecidos. Por eso, antes de instalar Manjaro definitivamente en mi PC de sobremesa, he decidido hacer una prueba.

He cogido un disco de 320GB que había desmontado de mi ordenador antiguo antes de deprenderme de él (con Windows Vista y en mi posesión desde 2007) tras 8 años de leal servicio y lo he montado en el actual, y así poder probar el sistema con todos los recursos reales del ordenador, con doble monitor y la gráfica Nvidia.

He decidido instalar tres juegos. <strong>Euro Truck Simulator 2</strong>, un simulador de camiones al que suelo jugar mucho y está disponible en Linux de forma nativa; <strong>The Division</strong>, es un shooter en tercera persona con una calidad gráfica suficiente para exprimir mi tarjeta gráfica y <strong>Rising Storm 2 Vietnam</strong>, últimamente mi FPS favorito, con un motor gráfico de hace bastantes años pero puramente multijugador.

Para saber el rendimiento en estos juegos usaré el marcador de FPS de Steam y la misma calidad gráfica en todas las pruebas.

![](/assets/2019/10/installing-games-manjaro-1024x640.png)
_Instalando Euro Truck Simulator 2 y The Division. Los otros dos paquetes se instalan automáticamente para la compatibilidad con Linux._

### Jugando en Windows

#### Euro Truck Simulator 2

Este juego no es que sea muy exigente gráficamente hablando, por lo que lo tengo a Ultra con algunas opciones en Alto. He realizado un trayecto pequeño, a penas 135km con lluvia y me mantenía unos 75 FPS estables. Nada mal.

#### The Division

Hacía mucho que no jugaba a este shooter, me trae buenos recuerdos a pesar de los terriblemente repetitivo que es. He completado algunas misiones llenas de disparos, explosiones, etcétera y ha estado entre los 50 y los 60 FPS en una calidad Alta.

#### Rising Storm 2 Vietnam

Como he dicho antes no se requiere un gran equipo para este juego, tiene un motor gráfico de hace años y no es que tenga una calidad extrema, lo juego a Ultra. Ya sea en tiroteos o en medio del fuego de artillería se ha mantenido a 60 FPS estables.

### Jugando con drivers de código abierto

#### Euro Truck Simulator 2

Me he puesto la misma configuración que jugando en Windows, todo en Alto y con lluvia. He de decir que es el recorrido de 15km más largo que he hecho, mantenía unos 9 FPS estables y alguna vez tenía un pico que llegaba a 15. Imposible jugar así.

#### The Division

Al inciar el juego ha marcado que estaba instalando DirectX de Windows. Tras esto ha estado varios minutos sin hacer nada y entonces ha aparecido la pantalla de inicio de sesión de Uplay, la plataforma de juegos de Ubisoft.

![](/assets/2019/10/the-division-microsoft-instalation.png)

Luego, al ver que no hacía nada reinicié el ordenador, volví a iniciar el juego y me fui a hacer otras cosas. Al volver sorpresa sorpresa tenía este mensaje en mi pantalla y todas las animaciones de Plasma estaban bugeadas.

![Mensaje de error en inglés. Traducido al español dice "Error al iniciar la renderización. La configuración inicial de la renderización ha fallado. Es posible que estés intentando acceder desde un dispositivo no soportado.](/assets/2019/10/render-error.png)

Tras este mensaje ya no ocurre nada. He mandado reporte a <a href="https://www.protondb.com/" target="_blank">ProtonDB</a>.

#### Rising Storm 2 Vietnam

Qué puedo decir. Tras tres intentos de iniciarlo no ocurre nada. Era de esperar teniendo en cuenta que aún en Windows su inicio suele ser lento y en alguna ocasión da problemas, se bugea o no responde.

### Jugando con drivers propietarios
#### Euro Truck Simulator 2

Con el temor de la prueba con los drivers abiertos aún en la sangre he dado una vuelta a una ciudad bajo la lluvia y con calidad Alta y el juego nunca bajaba de los 60 FPS iba de maravilla.

#### The Division

El juego ha arrancado correctamente, como si nada, y he podido jugar dos misiones pequeñas. Se mantiene en los 60 FPS con pequeños picos de 40 FPS en calidad Alta. Poniendo el juego en la calidad automática me mantiene los 60 FPS y baja de vez en cuando durante los tiroteos pero aún se puede jugar sin molestias.

#### Rising Storm 2 Vietnam

El juego ha tardado un poco en iniciar (como en Windows) pero al menos lo ha hecho. Ya el menú tenía lagazos importantes que cambiando el juego a "Ventana sin bordes" se soluciona. También el personaje que aparece en la portada estaba sin un buen procesado, le faltaban los ojos y los camuflajes no están bien. Tiene un problema grave y es que no es compatible con el sistema anti-hack, esto quiere decir que en menos de 5 minutos de haber empezado a jugar te echará de la partida. Para solucionar esto, la única opción que hay hasta que lo arreglen, es entrar en servidores que no tengan activado el sistema anti trampas, una gran minoría.

## Conclusión

Me entristece decirlo pero no usaría drivers libres para jugar, ni pensarlo, al menos de momento. Los drivers propietarios sí que permiten jugar de una forma placentera, no tanto como en Windows, pero sigue estando muy optimizado. Proton ha ayudado mucho a la instalación de los juegos y, repito, Valve ha hecho un gran trabajo con la plataforma Steam para darle juegos a Linux.

Aún hay cosas que pulir, los sistemas anti-trampas no están preparados para usarlos en Linux ni con Proton, por lo que los multijugadores que usan esto quedan descartados por ahora, una pena.

Aún quiero probar <a href="https://store.steampowered.com/app/494840/UBOAT/" target="_blank">UBOAT</a>, un juego indie en early access que estuve más de un año esperando y no me ha decepcionado, es algo inestable y tiene bugs, será un buen exámen para Proton. También, fuera de Steam, quiero probar Minecraft, ponerle shaders y paquetes de texturas.
