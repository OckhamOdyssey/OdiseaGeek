---
layout: post
title: Creando y añadiendo claves GPG a YubiKey
date: 2022-03-11 13:39:33.000000000 +01:00
type: post
parent_id: '0'
published: true
password: ''
status: publish
categories:
- Apuntes ASIR
- Seguridad y Alta Disponibilidad
tags:
- gpg
- linux
- pgp
- yubico
- yubikey

---

En esta publicación no vamos a entrar en detalles sobre qué es una YubiKey o una clave GPG, ni qué usos tienen, vamos a ir directos al grano. Pero explicando todos los procesos y lo que aparece en pantalla. Igualmente, si hay cosas que no se entienden agradecería que aviséis en los comentarios para corregirlo.

Primero de todo, las llaves YubiKey disponen de una contraseña de acceso y una de administración. La contraseña de administración se usa para añadir las claves, la de acceso para utilizarlas.

## Preparación

Primero de todo, vamos a asegurarnos que los paquetes que necesitamos están instalados:

<ul>
<li>ccid: Es el driver de tarjetas inteligentes.</li>
<li>gnupg o gpg: Normalmente, ya viene preinstalado en la mayoría de sistemas.</li>
<li>yubikey-personalization: Para cambiar algunos parámetros de la llave.</li>
<li>pinentry: Solicita las claves de la tarjeta para poder realizar la operación gpg.</li>
</ul>

Además, necesitamos habilitar el demonio de las tarjetas inteligentes.

```terminal
sudo systemctl start pcscd
sudo systemctl enable pcscd
```

## Contraseñas en la YubiKey

```
gpg --card-edit
```

Con el anterior comando estamos entrando en el módulo de claves GPG de la YubiKey, este módulo tiene su propio sistema de autentificación con contraseñas por defecto, por eso es muy importante cambiarlas. Primero, para poder realizar cambios cruciales en la tarjeta entraremos como administradores con el comando <code>admin</code>.

```
gpg/tarjeta> admin
Se permiten órdenes de administrador

```

| Nombre    | Valor por defecto | Uso                            |
|-----------|-------------------|--------------------------------|
| PIN       | 123456            | Acceso y uso de las claves GPG |
| Admin PIN | 12345678          | Modificación de las claves GPG |
| Reset PIN | N/A               | Resetear PIN                   |

Nos interesa cambiar al menos las dos primeras. Para ello, en el prompt de la Yubikey, escribimos <code>passwd</code>. Nos devolverá algo parecido a esto:

```
gpg/tarjeta> passwd
gpg: tarjeta OpenPGP num. D2760001240103040006166864620000 detectada

1 - change PIN
2 - unblock PIN
3 - change Admin PIN
4 - set the Reset Code
Q - quit

Su elección: 
```

En nuestro caso usaremos la opción 1 y la opción 3

## Creación de las claves

Ahora toca crear las claves. Para usar la YubiKey de la mejor forma generaremos un par de claves maestros, que nos servirán para firmar y certificar y del que colgarán dos subclaves, para encriptar y autenticar respectivamente.

Si no lo hemos hecho ya, salimos del prompt de la tarjeta. Ahora escribimos el comando para generar las claves.

```terminal
gpg --full-gen-key
```

La llave que crearemos tendrá las siguientes características:

<ul>
<li>El tipo de claves será RSA, la opción por defecto.</li>
<li>Tendrán un tamaño de 2048 bits. Las YubiKey no soportan un tamaño mayor.</li>
<li>Podemos poner la caducidad que queramos, para este ejemplo la pondré de 6 meses.</li>
<li>Se separarán las funciones de encriptar y autenticar en otras subclaves. Se hará más adelante</li>
</ul>

Voy a hacer una simulación creando la llave maestra, para que podáis ver el proceso completo.

```
gpg (GnuPG) 2.2.32; Copyright (C) 2021 Free Software Foundation, Inc.
This is free software: you are free to change and redistribute it.
There is NO WARRANTY, to the extent permitted by law.

Por favor seleccione tipo de clave deseado:
   (1) RSA y RSA (por defecto)
   (2) DSA y ElGamal
   (3) DSA (sólo firmar)
   (4) RSA (sólo firmar)
  (14) Existing key from card
Su elección: 1
las claves RSA pueden tener entre 1024 y 4096 bits de longitud.
¿De qué tamaño quiere la clave? (3072) 2048
El tamaño requerido es de 2048 bits
Por favor, especifique el período de validez de la clave.
         0 = la clave nunca caduca
      <n>  = la clave caduca en n días
      <n>w = la clave caduca en n semanas
      <n>m = la clave caduca en n meses
      <n>y = la clave caduca en n años
¿Validez de la clave (0)? 6m
La clave nunca caduca
¿Es correcto? (s/n) s

GnuPG debe construir un ID de usuario para identificar su clave.

Nombre y apellidos: Ockham Odyssey
Dirección de correo electrónico: admin@odiseageek.es
Comentario: 
Ha seleccionado este ID de usuario:
    "Ockham Odyssey <admin@odiseageek.es>"

¿Cambia (N)ombre, (C)omentario, (D)irección o (V)ale/(S)alir? v
Es necesario generar muchos bytes aleatorios. Es una buena idea realizar
alguna otra tarea (trabajar en otra ventana/consola, mover el ratón, usar
la red y los discos) durante la generación de números primos. Esto da al
generador de números aleatorios mayor oportunidad de recoger suficiente
entropía.
Es necesario generar muchos bytes aleatorios. Es una buena idea realizar
alguna otra tarea (trabajar en otra ventana/consola, mover el ratón, usar
la red y los discos) durante la generación de números primos. Esto da al
generador de números aleatorios mayor oportunidad de recoger suficiente
entropía.
gpg: clave 61D4104EAD00CCE7 marcada como de confianza absoluta
gpg: certificado de revocación guardado como '/home/ockham/.gnupg/openpgp-revocs.d/B12F58499135721C09A1EEA261D4104EAD00CCE7.rev'
claves pública y secreta creadas y firmadas.

pub   rsa2048 2022-03-10 [SC] [caduca: 2022-09-04]
      B12F58499135721C09A1EEA261D4104EAD00CCE7
uid                      Ockham Odyssey <admin@odiseageek.es>
sub   rsa2048 2022-03-10 [E] [caduca: 2022-09-04]
```

Ahora podemos ver las claves privadas que tenemos usando el siguiente comando:

```terminal
gpg -K
```

El resultado debería ser parecido al siguiente:

```
gpg: comprobando base de datos de confianza
gpg: marginals needed: 3  completes needed: 1  trust model: pgp
gpg: nivel: 0  validez:   2  firmada:   0  confianza: 0-, 0q, 0n, 0m, 0f, 2u
gpg: siguiente comprobación de base de datos de confianza el: 2022-09-04
/home/ockham/.gnupg/pubring.kbx
-------------------------------
sec   rsa2048 2022-03-10 [SC] [caduca: 2022-09-04]
      B12F58499135721C09A1EEA261D4104EAD00CCE7
uid        [  absoluta ] Ockham Odyssey <admin@odiseageek.es>
ssb   rsa2048 2022-03-10 [E] [caduca: 2022-09-04]
```

### Qué acabamos de hacer

Ahora bien, <strong>¿qué significa todo lo que pone y qué se supone que acabamos de hacer?</strong> Hemos creado la llave maestra, de la que colgarán las otras claves. Si nos intentamos imaginar un llavero, podemos pensar que tenemos solo la arandela y la llave del portal de un edificio. Podemos acceder pero no entrar en la casa ni bajar al garaje, por ejemplo. Veamos las líneas una por una:

<ul>
<li><strong>sec</strong> nos indica que es la clave privada (secreta), seguido del tipo y la longitud que le hemos indicado (<strong>rsa2048</strong>) y la <strong>fecha de creación</strong>. <strong>SC</strong> lo explicaré más adelante. Luego aparece la fecha de caducidad y un identificador de la clave privada, no es la clave en sí.</li>
<li><strong>uid</strong> significa es el identificador de la clave, el propietario. Empieza con el nivel de confianza que tenemos con ese usuario, si somos nosotros mismos los que hemos creado esa clave la confianza será absoluta.</li>
<li><strong>ssb</strong> es la subclave secreta. Todo lo que aparece es exactamente igual exceptuando la "E" entre corchetes.</li>
</ul>

Pasamos al significado de las palabras en corchetes. Las claves GPG pueden limitarse para usarse solo para ciertas cosas las cuales pueden ser <strong>firmar</strong> (S), <strong>certificar</strong> (C), <strong>encriptar</strong> (E) y <strong>autenticar</strong> (A). La llave maestra nos sirve para firmar documentos o certificar, que significa firmar otras claves. La certificación es utilizada principalmente para firmar nuestra siguiente clave GPG cuando la primera está a punto de caducar, y así el resto de usuarios puede asegurar que realmente es nuestra.

Como podemos ver, no tenemos ninguna llave para autenticar (en el ejemplo del llavero podríamos decir que es el tener la llave de nuestro apartamento). Ahora vamos a crearla.

### Creación de subclaves

Como ya tenemos la clave de encriptado, solo necesitamos crear la de autentificación. Primero accedemos al prompt para editar la clave.

```terminal
gpg --expert --edit-key
```

Si tenemos más de una clave, tendremos que añadir también el ID, normalmente pulsando dos veces el tabulador salen las opciones.

Ahora tendremos que usar el comando addkey para añadir una nueva subclave. El proceso será parecido a la creación de la llave maestra, será RSA  de 2048 bits, pero personalizaremos las propiedades para indicar que solo puede autenticar. También le daremos una fecha de caducidad (en este ejemplo he puesto sin caducidad).

```
gpg (GnuPG) 2.2.32; Copyright (C) 2021 Free Software Foundation, Inc.
This is free software: you are free to change and redistribute it.
There is NO WARRANTY, to the extent permitted by law.

Clave secreta disponible.

sec  rsa2048/61D4104EAD00CCE7
     creado: 2022-03-10  caduca: nunca       uso: SC  
     confianza: absoluta      validez: absoluta
ssb  rsa2048/DEC2EC761F6A9986
     creado: 2022-03-10  caduca: nunca       uso: E   
[  absoluta ] (1). Ockham Odyssey <admin@odiseageek.es>

gpg> addkey
Por favor seleccione tipo de clave deseado:
   (3) DSA (sólo firmar)
   (4) RSA (sólo firmar)
   (5) ElGamal (sólo cifrar)
   (6) RSA (sólo cifrar)
   (7) DSA (permite elegir capacidades)
   (8) RSA (permite elegir capacidades)
  (10) ECC (sólo firmar)
   (11) ECC (permite elegir capacidades)
   (12) ECC (sólo cifrar)
   (13) Clave existente
  (14) Existing key from card
Su elección: 8

Posibles accriones para una RSA clave: Firma Cifrado Autentificación 
Acciones permitidas actualmente: Firma Cifrado 

   (F) Conmutar la capacidad de firmar
   (C) Conmutar la capacidad de cifrado
   (A) Conmutar la capacidad de autenticación
   (S) Acabado

Su elección:
```

Lo siguiente es importante entenderlo bien. Conmutar significa cambiar de Sí a No o viceversa. Podemos ver arriba que las acciones actuales son Firma y Cifrado. Si seleccionamos la opción A se activará la opción de autenticar.

```
Posibles accriones para una RSA clave: Firma Certificar Cifrado Autentificación 
Acciones permitidas actualmente: Firma Certificar Autentificación 

   (F) Conmutar la capacidad de firmar
   (C) Conmutar la capacidad de cifrado
   (A) Conmutar la capacidad de autenticación
   (S) Acabado

Su elección:
```

Ahora si seleccionamos la F, como ya estaba activada por defecto, se desactivará.

```
Posibles accriones para una RSA clave: Firma Certificar Cifrado Autentificación 
Acciones permitidas actualmente: Certificar Autentificación 

   (F) Conmutar la capacidad de firmar
   (C) Conmutar la capacidad de cifrado
   (A) Conmutar la capacidad de autenticación
   (S) Acabado

Su elección:
```

Y hacemos lo mismo con la certificación, luego pulsamos S para continuar creando la clave.

```
las claves RSA pueden tener entre 1024 y 4096 bits de longitud.
¿De qué tamaño quiere la clave? (3072) 2048
El tamaño requerido es de 2048 bits
Por favor, especifique el período de validez de la clave.
         0 = la clave nunca caduca
      <n>  = la clave caduca en n días
      <n>w = la clave caduca en n semanas
      <n>m = la clave caduca en n meses
      <n>y = la clave caduca en n años
¿Validez de la clave (0)? 0
La clave nunca caduca
¿Es correcto? (s/n) s
¿Crear de verdad? (s/N) s
Es necesario generar muchos bytes aleatorios. Es una buena idea realizar
alguna otra tarea (trabajar en otra ventana/consola, mover el ratón, usar
la red y los discos) durante la generación de números primos. Esto da al
generador de números aleatorios mayor oportunidad de recoger suficiente
entropía.

sec  rsa2048/61D4104EAD00CCE7
     creado: 2022-03-10  caduca: nunca       uso: SC  
     confianza: absoluta      validez: absoluta
ssb  rsa2048/DEC2EC761F6A9986
     creado: 2022-03-10  caduca: nunca       uso: E   
ssb  rsa2048/6654BAD4D79CA815
     creado: 2022-03-10  caduca: nunca       uso: SEA 
[  absoluta ] (1). Ockham Odyssey <admin@odiseageek.es>
```

## Copia de seguridad de las claves secretas

Ahora que "el llavero" está completo podemos pasarlo a la YubiKey. Este proceso eliminará la clave privada de nuestro ordenador y la mantendrá solo en la YubiKey por lo que, si se extravía, ya no tendremos acceso a nuestra clave GPG. Para evitarlo, lo mejor será hacer una copia de seguridad y guardarla en otro lugar.

```terminal
gpg --export-secret-key --armor clave_secreta.asc</div>
```

```terminal
gpg --export-secret-subkeys --armor subclaves_secretas.asc
```

## Generar certificado de revocación

Si por alguna razón se nos ha extraviado la clave privada o nos la han robado debemos revocarla, indicarle a todo el mundo que ya no es nuestra clave e invalidarla. Esto se consigue con un simple comando:

```terminal
gpg --output revoke.asc --gen-revoke
```

## Mover las claves secretas a la YubiKey

Este es un proceso delicado si no hemos hecho el paso anterior, por eso vuelvo a insistir y recomiendo hacer una copia de seguridad de las claves antes. En esta fase se eliminarán las claves privadas y se almacenarán en distintas celdas de la tarjeta. Cada celda tiene un propósito; firmar, encriptar y autenticar; por eso hemos creado las subclaves. De esta forma la llave sabrá qué clave usar en qué momento. Por su parte, el sistema generará una referencia de nuestro llavero, indicando que las claves se almacenan en un dispositivo físico (smart card) y que debe ser insertada para utilizarse. La clave nunca se almacenará en el ordenador.

```terminal
gpg --edit-key
```

Veremos un menú parecido al siguiente:

```
gpg (GnuPG) 2.2.32; Copyright (C) 2021 Free Software Foundation, Inc.
This is free software: you are free to change and redistribute it.
There is NO WARRANTY, to the extent permitted by law.

Clave secreta disponible.

sec  rsa2048/61D4104EAD00CCE7
     creado: 2022-03-10  caduca: nunca       uso: SC  
     confianza: absoluta      validez: absoluta
ssb  rsa2048/DEC2EC761F6A9986
     creado: 2022-03-10  caduca: nunca       uso: E   
ssb  rsa2048/6654BAD4D79CA815
     creado: 2022-03-10  caduca: nunca       uso: A   
[  absoluta ] (1). Ockham Odyssey <admin@odiseageek.es>
```

Por defecto está seleccionada la clave maestra, como ya vimos antes, es la que pone sec.

<strong>Aviso que como ya tengo mis claves en la tarjeta no he reproducido de nuevo los siguientes pasos, así que voy a sacar los ejemplos de la documentación.</strong>

Escribimos en el prompt <strong><code>keytocard</code></strong> para enviarla a la tarjeta y seguimos los pasos que nos indica. La clave maestra es importante moverla a la celda de firma (signature) porque ya tenemos otra clave específica para la autentificación.

```
gpg> keytocard
Really move the primary key? (y/N) y
Signature key ....: [none]
Encryption key....: [none]
Authentication key: [none]
Please select where to store the key:
   (1) Signature key
   (3) Authentication key
Your selection? 1
gpg> key 1
sec 2048R/13AFCE85 created: 2014-03-07 expires: 2014-06-15
                   card-no: 0000 00000001
ssb* 2048R/D7421CDF created: 2014-03-07 expires: never
ssb  2048R/B4000C55 created: 2014-03-07 expires: never
(1)  Foo Bar <foo@example.com>
```

Como también podemos ver en la captura de arriba, ahora aparece una línea que indica el número de nuestra YubiKey. Ahora, para guardar las subclaves usamos el comando key seguido del número de subclave por orden, siendo la primera ssb la key 1. La clave seleccionada aparecerá marcada con un asterisco.

Con esto ya podemos añadir el resto de claves a sus respectivas secciones, no os lo voy a dar todo mascado.

Con el comando <code>quit</code> salimos y, obviamente, guardamos los cambios.

```
gpg> quit
¿Grabar cambios? (s/N) s
```

## Opcional: Requerir tocar la llave para usar las claves

Una de las cualidades de la YubiKey es que tiene una zona táctil, requiere pulsar está para comenzar con el proceso. Esto añade una capa de seguridad física, al hacer necesaria la interacción del usuario, es imposible que un hacker acceda a nuestra clave privada aunque la llave esté conectada y tenga acceso completo a nuestro ordenador.

Para hacer esto primero necesitamos asegurarnos que tenemos el sistema de personalización de YubiKey que hemos instalado al principio. Ahora, con un comando, podemos habilitar la opción para la firma, la autentificación o la encriptación.

### Firma

```terminal
ykman openpgp keys set-touch SIG on
```

### Encriptado

```
ykman openpgp keys set-touch ENC on
```

### Autenticación

```terminal
ykman openpgp keys set-touch AUT on
```

## Comprobación

Ahora podemos comprobar que las claves han cambiado de lugar con un simple comando que ya hemos visto.

```terminal
gpg -K
```

La salida será parecida a la siguiente.

```
/home/ockham/.gnupg/pubring.kbx
-------------------------------
sec>  rsa2048 2022-03-10 [SC] [caduca: 2022-09-04]
      B12F58499135721C09A1EEA261D4104EAD00CCE7
uid        [  absoluta ] Ockham Odyssey <admin@odiseageek.es>
ssb>  rsa2048 2022-03-10 [E] [caduca: 2022-09-04]
ssb>  rsa2048 2022-03-10 [A] [caduca: 2022-09-04]
```

La diferencia es que en las claves ahora aparece el símbolo de mayor qué, esto significa que la clave no se encuentra en el ordenador, sino en la smart card. Ahora vamos a probar a firmar algo, un texto muy básico.

```terminal
echo "test" | gpg --clearsign
```

Si la tarjeta no está conectada aparecerá lo siguiente:
```
-----BEGIN PGP SIGNED MESSAGE-----
Hash: SHA256

test
gpg: firma fallida: La tarjeta no está
gpg: [stdin]: clear-sign failed: La tarjeta no está
```

Si la tarjeta está introducida se nos solicitará el código pin de la tarjeta, no es el de administrador, y aparecerá la firma. Recordad que si habéis seguido los pasos del punto anterior tendréis que tocar la tarjeta para que se efectúe la firma.

```
-----BEGIN PGP SIGNED MESSAGE-----
Hash: SHA256

test
-----BEGIN PGP SIGNATURE-----    

iQEzBAEBCAAdFiEE1lrNvaqfsNqeAgPEEA7DlDqoGQ0FAmIrNKMACgkQEA7DlDqo
GQ3WFggAnCUW4mq1Vfn9IcNQMkAuWI3o8JJ/xpG7a7K6GbXl/vhrk57qzegwWGc5
t3jTmcNqABqdJgqabG0rmiMKWfVMsjMAqXsjTsSjiuVmXuyGcLH0jUEjJLMiZuJt
htIvgKdr1gUb9umITatqtZa2IzsChB8H3fYXYRoOU/hisWZ1zV+L3AkBpTXLsP23
aDU0TLxaEWrOBytfVR5Q84d6zVbi5Bqoo0MEsyg7r/oXUDLXF+9rgBpCTmIFv3/o
JYl2ZdFdrrrXlaGJUgkbBSmHwgNiZOFibRu5BR66FVD86HnOHoAl05mvK/BGjrns
KEGh24zUUdGrKqABoyEwEsDDN1CGCw==
=g6jR
-----END PGP SIGNATURE-----
```

Esta guía se ha hecho basándose en la experiencia propia, la <a href="https://developers.yubico.com/PGP/PGP_Walk-Through.html" target="_blank">documentación de Yubico</a> y la <a href="https://github.com/drduh/YubiKey-Guide" target="_blank">guía de <em>drduh</em> en Github</a>.