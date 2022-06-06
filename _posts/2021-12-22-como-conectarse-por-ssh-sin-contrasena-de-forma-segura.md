---
layout: post
title: Cómo conectarse por SSH sin contraseña de forma segura
date: 2021-12-22 10:38:41.000000000 +01:00
type: post
parent_id: '0'
published: true
password: ''
status: publish
categories:
- Sin categoría
tags: []
---
Durante la gestión de un servidor es importante mantener cierta seguridad y la mejor forma de evitar que descubran la contraseña de acceso es no tener contraseña. El servicio SSH permite identificar al cliente usando un par de claves criptográficas.

Primero debemos crear desde el cliente las claves que usaremos, ssh nos ofrece una herramienta justo para eso.

```terminal
ssh-keygen -t rsa -b 2048 -f nombre
```

Se nos creará la clave privada `nombre` y la clave pública `nombre.pub` de encriptación RSA y tamaño 2048 (no es recomendable usar un tamaño menor).

La clave privada nos identificará en el servidor y la clave pública le indicará al servidor que realmente somos nosotros. Para añadir a nuestra identificación SSH la clave privada ejecutamos el siguiente comando:

```terminal
ssh-add nombre
```

Y ahora toca añadir a los servidores que queramos la clave.

```terminal
ssh-copy-id -i nombre usuario@server
```

Nos pedirá la contraseña de acceso al servidor y se copiará.

Ahora bien, podemos eliminar la posibilidad de iniciar sesión con contraseña, de forma que solo los usuarios@equipos que hayan seguido este procedimiento y el servidor tenga su clave pública podrán conectarse. Para hacer esto debemos editar el fichero de configuración del servicio ssh.

```terminal
sudo nano /etc/ssh/sshd_config
```

Y debemos añadir la siguiente línea

```
PasswordAuthentication no
```

Recargamos el servidor y ya estará todo listo

```terminal
sudo systemctl reload sshd
```