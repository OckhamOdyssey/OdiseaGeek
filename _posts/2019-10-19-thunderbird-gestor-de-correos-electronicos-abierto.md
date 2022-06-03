---
layout: post
title: Thunderbird. Gestor de correos electrónicos abierto.
date: 2019-10-19 14:57:49.000000000 +02:00
type: post
parent_id: '0'
published: true
password: ''
status: publish
categories:
- Misceláneo
tags:
- google
- KDE
- KMail
- linux
- Mozilla
- OAuth
- Outlook
- thunderbird
- Windows
---
He de ser sincero. Este artículo lo creé a consta del de [KMail]({% post_url 2019-10-13-vincular-una-cuenta-de-gmail-en-kmail %}). Quise usar un gestor de correo electrónico libre y pensé que el que viene incluido en KDE sería buena idea. Los problemas que tiene con Gmail como muestra el artículo no son los únicos, durante mi corta experiencia con este gestor he sufrido de congelamientos constantes (incluso en otras [TTY]({% post_url 2019-10-09-que-es-el-cli-y-el-gui%}), he tenido que volver a hacerlo todo varias veces y hasta un par de reinicios. Otros compañeros han tenido también problemas similares. Simplemente horrible.

Aproveché esto para probar Mozilla Thunderbird, el gestor de correos electrónicos más conocido, de código abierto, bajo licencia libre de Mozilla (compatible con GPL) y disponible en Linux, Windows y Mac. A demás es el que se usa en la empresa en la que estoy haciendo prácticas.

## Instalación

Como siempre, antes de todo, vamos a ver como se instala este programa.

### Windows

Puedes instalar la última versión para Windows a través de <a href="https://www.thunderbird.net/es-ES/" target="_blank">este enlace</a>. Si no te descarga la versión con tu idioma o la versión correcta puedes buscarla <a href="https://www.thunderbird.net/es-ES/thunderbird/all/" target="_blank">aquí</a>. La opción personalizada no es muy larga ni complicada por lo que recomiendo usar esta.

### Ubuntu

Empezamos con la actualización de los repositorios seguido de la descarga del programa y del paquete en español.

```terminal
sudo apt-get update -y
sudo apt-get install thunderbird thunderbird-locale-es-es -y
```

Tras esto ya se puede abrir el programa tranquilamente.

## Configurar una cuenta de correo electrónico

Si es la primera vez que abrimos el programa nos saldrá una ventana emergente donde crearemos la cuenta. Para cualquier otra circunstancia podremos hacer clic derecho en el panel lateral izquierdo y seleccionar "<strong>Configuración</strong>". En la parte de abajo podremos ver un desplegable "<strong>Operaciones sobre la cuenta</strong>" donde escogemos "Añadir cuenta de correo".

![](/assets/2019/10/instituto-correo.png)
_Las opciones de inicio de sesión son más simples y están más automatizadas._

Al darle a "<strong>Continuar</strong>" Thunderbird intentará encontrar la configuración del servidor de correo seleccionado. Por la experiencia que he tenido he visto que tiene muchas más listas automáticas de las que esperaba, incluido el servidor de correo de mi instituto, en este caso ya está todo hecho. Si no encuentra el servidor primero revisa que esté todo bien escrito, luego coloca manualmente el servidor entrante y saliente o consulta en Internet o al encargado IT los servidores.

![](/assets/2019/10/gsuite-thunderbird.png)
_Si la cuenta es de Gmail o está bajo gestión de G-Suite se abrirá una pestaña solicitando el acceso._

Y con esto la cuenta ya está creada, notoriamente más fácil que en KMail.

A diferencia de KMail, el envío de correos, las firmas y la gestión de mensajes en general es mucho más intuitivo y no considero que se necesite más información de la que ya se ha dado. Escribe un comentario si no crees que sea así.

![](/assets/2019/10/bandeja-de-entrada.png)
_Thunderbird no es como Gmail o Outlook, no forma parte de un servicio de correo así que tiene la misma compatibilidad con todos y puede gestionarlos por igual._

En la imagen de arriba podemos ver distintos servicios de correos que se gestionan a través de este cliente. Gmail es muy conocido y sencillo de agregar, como hemos visto arriba se abre una ventana para autorizar a través de la OAuth de Google. Después podemos ver una cuenta de correo de este dominio, por razones obvias no detecta qué configuración usa, cambiando dos opciones tal y como aparecen en el hosting podremos acceder sin problema. La tercera opción es de Vivaldi, un navegador web que usa su propia cuenta de correo electrónico para las copias de seguridad, este servicio de correo no es tan conocido pero aun así Thunderbird lo configura automáticamente. El siguiente es una cuenta privada gestionada por G-suite, también la ha detectado automáticamente.