---
title: Usando la clave privada GPG de Yubikey en un nuevo ordenador
date: 2022-12-18 18:47:07.000000000 +02:00
status: publish
categories:
- Misceláneo
tags:
- gpg
- linux
- pgp
- yubico
- yubikey
- git
- github
---
Supongamos que hemos seguido los pasos para [crear y añadir claves GPG a YubiKey]({% post_url 2022-03-11-creando-y-anadiendo-claves-gpg-a-yubikey %}) en un ordenador y nos ha ido todo bien. Las claves y subclaves privadas están almacenadas en nuestro dispositivo y podemos comprobarlo con un `gpg -K`.

```
/Users/ockham/.gnupg/secring.gpg
------------------------------------
sec>   4096R/3AA5C34371567BD2 2022-03-08 [SC] [caduca: 2022-09-04]
       Número de serie de la tarjeta = 0006 16752384
uid               [  absoluta ] Ockham Odyssey <admin@odiseageek.es>
ssb>   4096R/42B317FD4BA89E7A 2022-03-08 [A] [caduca: 2022-09-04]
ssb>   4096R/44BF8217A52AF753 2022-03-08 [E] [caduca: 2022-09-04]
```

Ya sabemos que el símbolo `>` significa que esas claves se encuentran en la Yubikey pero, ¿qué pasa si queremos usar las claves en un nuevo ordenador? Aunque conectemos la llave no nos aparecerán las claves. Para solucionarlo solo hay que lanzar unos pocos comandos.

Primero hay que comprobar si tenemos nuestra clave pública.

```terminal
gpg -k
```

Si no tenemos la clave pública no podemos continuar. La clave púbica la obtendremos de un servidor de claves o del anterior ordenador.

Una vez tengamos la clave pública en nuestro ordenador, podemos usar el comando `gpg --card-status` para ver la vinculación de la clave privada de la Yubikey con la clave pública que tenemos en nuestro llavero.

```
General key info..: pub  rsa2048/61D4104EAD00CCE7 2022-03-10 Ockham Odyssey <admin@odiseageek.es>
sec>  rsa2048/61D4104EAD00CCE7  creado: 2022-03-10  caduca: 2022-09-04
                                num. tarjeta: 0006 16686462
ssb>  rsa2048/A727D9E9D662D0F4  creado: 2022-03-10  caduca: 2022-09-04
                                num. tarjeta: 0006 16686462
ssb>  rsa2048/603701C9D96673AA  creado: 2022-03-10  caduca: 2022-09-04
                                num. tarjeta: 0006 16686462
```

Si en `General key info` vemos un `[ none ]` es que la clave pública que hemos pasado no es la correcta.

Cuando veamos algo parecido a lo de arriba, entramos en las opciones de la llave con `gpg --card-edit` y escribimos el comando `fetch`. Esto añadirá las claves GPG privadas a nuestro llavero. Salimos con de la terminal de GPG con un `quit`.

Si ejecutamos de nuevo `gpg -K` veremos que ahora sí que aparecen las claves de nuestra Yubikey.