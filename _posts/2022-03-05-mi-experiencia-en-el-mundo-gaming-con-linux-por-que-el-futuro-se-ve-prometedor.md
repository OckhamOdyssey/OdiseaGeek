---
layout: post
title: Mi experiencia en el mundo gaming con Linux. Por qué el futuro se ve prometedor
date: 2022-03-05 16:31:29.000000000 +01:00
type: post
parent_id: '0'
published: true
password: ''
status: publish
categories:
- Misceláneo
tags:
- Arch Linux
- gaming
- juegos
- KDE
- linux
- Manjaro
- Proton
- ProtonDB
- Steam
- Valve
---

Es innegable que Windows tiene una aplastante cuota de mercado en el mundo gaming, siendo prácticamente inexistente en Linux o Mac. Pero también hay que destacar que en los últimos años el gaming en Linux ha hecho el mayor avance desde sus inicios, y es que Valve, la empresa detrás de Steam, es gran responsable de esto. Para aquellos que no estén muy metidos en el tema vamos a ponernos en situación.

## Wine, Valve, Proton y SteamDeck

<strong><a href="https://www.winehq.org" target="_blank">Wine</a></strong> es un programa de bajo nivel que permite la compatibilidad de aplicaciones de Windows en los sistemas Linux de una forma bastante peculiar, y es que no es un emulador, sino una capa intermedia entre la aplicación y Linux (es más, su nombre es el <a href="https://es.wikipedia.org/wiki/Acr%C3%B3nimo_recursivo" target="_blank">acrónimo recursivo</a> de Wine Is Not Emulator). Su creación empezó a finales de los 90 y a día de hoy sigue actualizándose periódicamente detrás de una gran comunidad.

El problema de Wine es que es complejo, y cada aplicación requiere unos parámetros y unas dependencias concretas. Aun así, muchos siguen dando problemas para ejecutarse por el Framework .NET de Microsoft. Y ahí es donde entra Valve, y es que la empresa detrás de la mayor plataforma de videojuegos para PC está luchando contra viento y marea para sacar adelante su proyecto <strong>Proton</strong>. Este es una versión de Wine adaptada para videojuegos y, aunque su software no tiene una licencia permisiva, mantiene el código abierto en un <a href="https://github.com/ValveSoftware/Proton" target="_blank">repositorio público de GitHub</a>.

Otro gran esfuerzo por parte de Valve ha sido la creación de su consola, la llamada Steam Deck, muy familiar a la Nintendo Switch. La Steam Deck es básicamente un ordenador de bolsillo capaz de correr una gran cantidad de juegos de Steam aunque con muy poca autonomía. Su sistema operativo, el SteamOS, está basado en ArchLinux, lo que ha hecho que los sistemas antitrampas y los desarrolladores de videojuegos tengan una presión extra a la hora de mantener la compatibilidad con Linux.

![Una persona jugando Stardew Valley en un Steam Deck](/assets/2022/03/playing_stardew.jpg)
_Stardew Valley en Steam Deck_

## Los sistemas antitrampas

Un pequeño gran dolor de cabeza en el mundo Linux son los sistemas antitrampas de los juegos, y es que EasyAntiCheat y BattleEye están empezando a implementar ahora compatibilidad con Linux no sin problemas, y es que al parecer los antichetos siguen saltando dependiendo del kernel que se tenga instalado. Igualmente, ahora corresponde a los desarrolladores aplicar los cambios necesarios para esta compatibilidad, aunque el juego en sí no tenga soporte en Linux, Proton hará el resto.

## Mi experiencia jugando

Hace poco más de dos años publiqué <a href="https://www.odiseageek.es/probando-juegos-en-manjaro-kde/">una entrada</a> haciendo una pequeña comparativa con los juegos con los que más perdía el tiempo en esa época (antes del covid, no me lo creo), viendo su rendimiento en Windows, Linux con drivers de NVIDIA libres y los drivers propietarios. He de decir que me alegra que muchos de esos juegos ya funcionan genial.

Aunque el tiempo haya pasado sigo usando mi querido Manjaro KDE y, aunque la gráfica sigue siendo la misma RTX 2060 SUPER, el procesador lo he cambiado de un i5-8500 a un Ryzen 5 3600.

Algunos juegos que he probado aparecen como que tienen soporte nativo para Linux, como el Civilization VI, directamente ni arrancan. Los Metro 2033 y Metro Last Light también tiene soporte nativo, pero la calidad gráfica es peor y los FPS no suben de 30. A demás se me abren en la pantalla secundaria. La versión de Windows con Proton tira que no veas, todo al máximo y fluido. Juegos como el Rising Storm no lo he vuelto a probar en Linux, pero por lo que he visto su EAC aún nos echa de partida. El mítico Stardew Valley es el único que me va genial nativo, y ni lo he probado con Proton, la verdad es que  es de los mejores juegos que he probado y me sigue flipando que lo haya hecho una sola persona.

Si Portals y Halo hubieran tenido un hijo habría sido Splitgate, un juego frenético de disparos y portales. Es nativo aunque lo noto con pequeños tirones, seguro que con Proton va de lujo.

Ahora mismo mi lista de juegos descargados en Linux es muy corta, dadme tiempo, pero estoy vigilando algunos en la web de <a href="https://www.protondb.com" target="_blank">Protondb</a> para ver su compatibilidad.

<ul>
<li>Fallout 4: Categoría Oro y verificado para Steam Deck.</li>
<li>Rising Storm 2 Vietnam: Injugable por el antichetos.</li>
<li>Firewatch: Nativo</li>
<li>Star Wars Jedi Fallen Order: Categoría Oro y verificado para Steam Deck</li>
<li>Bioshock Infinite: Nativo</li>
<li>Dead by Deadlight: Injugable por el antichetos.</li>
<li>Fallout 76: Categoría Oro</li>
<li>GTA IV: Categoría Oro</li>
<li>The División: Categoría Plata</li>
</ul>

### Queda la Epic Store

Primero de todo hay que decir que la Epic Store no está disponible para Linux, pero eso no significa que no podamos jugarlos. Los fans han creado un lanzador de juegos llamado Heroic Games Launcher. Con este, podemos acceder a los juegos de nuestra biblioteca de Epic y descargarlos junto a una versión de Wine o Proton compatible. ¿A que es genial? Por mi parte tengo ahí el Subnáutica, el Red Dead Redemption y el Knockout City (todos con categoría oro) y la mar de contento.

### Emuladores por doquier

Nunca he sido muy fan de Nintendo, especialmente teniendo en cuenta sus reciclajes y su pésimo trato a los fans. Pero he de reconocer que tiene buenos juegos. Los Animal Crossing siempre los quise probar de pequeño y, aunque solo jugué a uno, los Pokemon también me han llamado la atención. Para ello, usando los emuladores de software libre <strong>Desmume</strong> y Citra, he podido probar estos juegos obtenidos de manera totalmente legal. A demás su soporte en Linux me parece tremendo. Cabe decir que Citra ahora también está disponible en la Play Store de Android.