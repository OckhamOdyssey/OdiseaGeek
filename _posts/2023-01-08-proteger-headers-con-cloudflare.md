---
layout: post
title: Proteger las cabeceras HTTP con Cloudflare
date: 2023-01-08 12:37:54 +02:00
type: post
published: true
password: ''
status: publish
categories:
- Seguridad Informática
tags:
- cloudflare
- http
- headers
---
Los servicios web expuestos son una causa frecuente de intentos de ataque. El uso de exploits, XSS, inyecciones SQL, páginas mal protegidas, robo de cookies… En fin, es importante proteger estos puntos de acceso a nuestros sistemas. A nivel programático deben gestionarse bien los sistemas de autenticación, páginas protegidas y posibles inyecciones, pero nosotros, como administradores de los sistemas, también podemos y debemos reducir estos riesgos.
Las cabeceras HTTP, o headers, envían información entre el cliente y servidor. Por parte del cliente, puede mandar datos como su navegador, sistema operativo o una cookie de sesión. El servidor generalmente utiliza los headers para indicarle al navegador qué JS o CSS puede ejecutar, el tipo de servidor y la versión de este, entre otras cosas.

# Headers de seguridad
Los servidores web deben configurarse para mandar ciertas cabeceras y protegerse a sí mismos y a los clientes de posibles ataques. Puedes saber qué headers envía tu servidor empleando páginas como [https://securityheaders.com](https://securityheaders.com/).

![](/assets/2023/01/mala-cabecera.png)

Esta página te muestra las cabeceras inseguras, qué implican y cómo solventarlas. Nosotros vamos a hacer una planificación con una seguridad base generalista.
- `Referrer-Policy`: Indica si el navegador debe mostrar la página de origen al hacer clic a un enlace. Se usa principalmente para analíticas pero puede haber páginas que requieran de esta función.
- `Permissions-Policy`: Es la nueva versión del `Feature-Policy`, solo cambia el nombre. Esta cabecera le dice al navegador qué permisos pueden solicitar las páginas. De esta forma, si un servidor es vulnerado y el atacante intenta solicitar permisos de cámara y micrófono del navegador del cliente, esta header puede evitar que lo logre.
- `Content-Security-Policy`: Hace referencia a los JS y CSS que se permiten ejecutar. Limita el protocolo y la ubicación de su procedencia. Por ejemplo, puede hacer que no se permita la ejecución de JavaScript externo a la propia página y que siempre deba ir por HTTPS.
- `X-Content-Type-Options`: Evita que los navegadores acepten cualquier tipo de contenido y fuerzas el uso de los MIME declarados por le servidor. Solo tiene el valor `nosniff`.
- `X-Frame-Options`: Restringe que el servidor pueda ser añadido como `iframe` en una página fraudulenta. Evita ataques de clickjacking.
- `server`: Muestra el servidor web y su versión. Puede ser peligroso porque puede existir una vulnerabilidad conocida para ese servidor, debe modificarse por otro valor.
- `Strict-Transport-Security`: Fuerza el uso de TLS en la conexión. El navegador guarda esta header y no permitirá la conexión con la web si deja de usar TLS. La cantidad de tiempo que permanecerá el valor guardado por le navegador se especifica como valor.

Añadiendo estas cabeceras habremos aumentado enormemente la seguridad de nuestro servidor y de los clientes que lo visitan.

![](/assets/2023/01/buena-cabecera.png)

## Headers básicos de ejemplo

Debes leer la documentación y ver que headers van a ser necesarios y que política deben cumplir. Añadir una política demasiado restrictiva puede hacer que el sitio deje de funcionar correctamente. En [este enlace][def] se da una guía más detallada de qué opciones tiene cada header. Vamos a dar un ejemplo básico:
- `Referrer-Policy: strict-origin-when-cross-origin`: El navegador enviará la URL completa a las solicitudes con el mismo origen, pero solo enviará el origen cuando las solicitudes sean de orígenes externos.
- `Permission-Policy: geolocation=()`: El navegador solo permitirá que el servidor solicite permisos para la geolocalización.
- `Content-Security-Policy: frame-ancestors 'self'; frame-src 'self' object-src 'self'; base-uri 'self'`: Permite los iframes que vienen de la propia página y de las páginas relacionadas con esta. [Content-Security-Policy CheatSheet][def2]
- `X-Content-Type-Options: nosniff`: Es el único valor posible.
- `X-Frame-Options: SAMEORIGIN`: Solo permite iframes del mismo servidor.
- `server: Yes`: Se puede poner cualquier valor siempre y cuando no de información sobre el servidor.
- `Strict-Transport-Security: max-age=31536000; includeSubDomains`: Fuerza el uso de TLS para el dominio y subdominios durante un año.

[def]: https://scotthelme.co.uk/hardening-your-http-response-headers/
[def2]: https://scotthelme.co.uk/csp-cheat-sheet/

# Implementar headers
Ahora que tenemos nuestras cabeceras de seguridad según nuestras necesidades, es hora de implementarlas. Dependiendo de la infraestructura que tengamos usaremos una herramienta u otra. Los headers pueden ponerse en los propios servidores web (Apache y Nginx) o en servicios de proxy inverso (Traefik y Cloudflare).

## Headers en Cloudflare
Puedes modificar los headers en Cloudflare independientemente de si tu cuenta es gratuita o de pago. Con la gratuita tienes un máximo de 10 **transformaciones** pero puedes usar un para modificar todas las cabeceras que quieras. Si quieres añadir filtros para que solo se apliquen a ciertas páginas entonces sí que tendrás que gastar el resto de transformaciones.

Las transformaciones puedes encontrarlas en **Rules** > **Transform Rules** > **HTTP Response Header Modification**.

![](/assets/2023/01/cloudflare-headers.png)

## Headers en Traefik
Si usas las labels de los contenedores Docker para exponer tus servicios estás de suerte, las cabeceras también pueden editarse individualmente para cada servicio usando esta herramienta. Un ejemplo sencillo sería el siguiente:

```yaml
labels:
    - "traefik.http.middlewares.<Nombre_del_servicio>.headers.customresponseheaders.X-Frame-Options=SAMEORIGIN"
```

Si, por otro lado, prefieres editar directamente el archivo YAML de configuración de Traefik puede hacerse añadiendo lo siguiente:

```yaml
http:
  middlewares:
    <Nombre_del_servicio>:
      headers:
        customResponseHeaders:
          X-Frame-Options: "SAMEORIGIN"
```

He de decir que solo he probado la primera opción. Para implementarlo en Kubernetes, Rancher, Marathon o configurarlo usando el TOML de Traefik, toda la documentación oficial se encuentra en [este enlace][def3]

## Headers en Apache
Si queremos configurar los headers en Apache primero debemos activar el módulo correspondiente.

```terminal
sudo a2enmod headers
```

Ahora se edita el fichero de configuración del sitio al que le queremos añadir los headers, usualmente en `/etc/apache2/sites-enabled/archivo_configuración.conf`, añadimos una línea por cada cabecera, tienen la siguiente estructura. [Documentación oficial con las opciones de la directiva Header][def4]

```
Header always set X-Frame-Options "SAMEORIGIN"
```

Recordar que hay que reiniciar el servidor tras estos cambios.

```terminal
sudo systemctl restart apache2
```

## Headers en Nginx

Dentro del archivo de configuración de Nginx en `/etc/nginx/nginx.conf` se pueden añadir líneas para añadir, eliminar y modificar headers. Aquí un ejemplo.

```
add_header X-Frame-Options SAMEORIGIN;
```

Tras esto, hay que reiniciar el servicio.

```terminal
sudo systemctl restart nginx
```

[def3]: https://doc.traefik.io/traefik/middlewares/http/headers/#configuration-examples
[def4]: https://httpd.apache.org/docs/2.4/mod/mod_headers.html#Header