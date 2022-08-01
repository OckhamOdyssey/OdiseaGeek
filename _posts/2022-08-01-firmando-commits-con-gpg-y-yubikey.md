---
title: Firmando commits con GPG y Yubikey
date: 2022-08-01 18:51:55.000000000 +02:00
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

> En esta entrada no vamos a explicar cómo generar claves GPG para la Yubikey, para ello tenemos esta otra: [Creando y añadiendo claves GPG a YubiKey]({% post_url 2022-03-11-creando-y-anadiendo-claves-gpg-a-yubikey %})
{: .prompt-info }

Ya hemos visto en otras entradas el gran potencial que tienen las Yubikey. Esta ocasión va a ser algo de gran interés para aquellos desarrolladores que no paran de hacer commits en git. Vamos a usar la Yubikey para firmar todos los commits que hagamos. Según el servidor git, podremos hacer que, si no se han firmado, se marquen como "No validados".

# Pasos previos

Partiremos por la base del anterior post y asumiremos que ya tenemos todas nuestras claves generadas y guardadas en la llave. Por lo que, al conectar la Yubikey al ordenador y ejecutar la orden `gpg -K` nos debería devolver algo parecido a lo siguiente:

```
/home/ockham/.gnupg/pubring.kbx
-------------------------------
sec>  rsa2048 2022-03-10 [SC] [caduca: 2022-09-04]
      B12F58499135721C09A1EEA261D4104EAD00CCE7
uid        [  absoluta ] Ockham Odyssey <admin@odiseageek.es>
ssb>  rsa2048 2022-03-10 [E] [caduca: 2022-09-04]
ssb>  rsa2048 2022-03-10 [A] [caduca: 2022-09-04]
```

Si no es así, se tendrá que seguir la otra entrada del blog antes de continuar.

# Configurando git

Primero de todo buscaremos nuestra clave privada:
```terminal
gpg --list-secret-keys --keyid-format=long
```
Recibiremos una respuesta como la siguiente:

```
/Users/ockham/.gnupg/secring.gpg
------------------------------------
sec>   4096R/3AA5C34371567BD2 2022-03-08 [SC] [caduca: 2022-09-04]
       Número de serie de la tarjeta = 0006 16752384
uid               [  absoluta ] Ockham Odyssey <admin@odiseageek.es>
ssb>   4096R/42B317FD4BA89E7A 2022-03-08 [A] [caduca: 2022-09-04]
ssb>   4096R/44BF8217A52AF753 2022-03-08 [E] [caduca: 2022-09-04]
```

Como buscamos firmar, seleccionaremos la primera clave (`3AA5C34371567BD2`) y ejecutaremos el siguiente comando para indicar a git que esa será nuestra clave de firma:

```terminal
git config --global user.signingkey 3AA5C34371567BD2
```

Ahora configuraremos git para que firme todos los commits por defecto:
```terminal
git config --global commit.gpgsign true
```

Y ya está. Ahora, a la hora de hacer un commit, según hayamos configurado la tarjeta, nos solicitará ciertos pasos. Si hemos seguido el orden del posts anterior, primero tendremos que dar la contraseña de la Yubikey para firmar y luego tendremos que pulsarla. El comando git se mantendrá a la espera de estos pasos para comenzar a firmar.

# Configurando GitHub

Si queremos que GitHub tenga en cuenta nuestras firmas tendremos que ir a la [configuración de claves SSH y GPG](https://github.com/settings/keys). Pegaremos la clave pública que podemos obtener tras ejecutar `gpg --armor--export 3AA5C34371567BD2`.

# Comprobación

Una vez hayamos hecho un commit (podemos hacerlo sin el `-S` para firmar si hemos configurado que firme por defecto) podremos verificar que se ha firmado correctamente ejecutando la siguiente orden:

```terminal
git show HEAD --show-signature
```

Tras hacer un push en GitHub, también podremos comprobar que reconoce la firma y marca el commit como verificado.

![GitHub marcando el commit como verificado](/assets/2022/08/git-gpg-sign.png)
