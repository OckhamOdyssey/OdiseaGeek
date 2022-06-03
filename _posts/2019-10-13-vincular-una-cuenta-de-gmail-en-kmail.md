---
layout: post
title: Vincular una cuenta de Gmail en KMail
date: 2019-10-13 16:12:33.000000000 +02:00
type: post
parent_id: '0'
published: true
password: ''
status: publish
categories:
- Misceláneo
tags:
- correo
- distro
- electrónico
- Gmail
- google
- imap
- KDE
- KMail
- Kontact
- linux
- Plasma
- pop
---
> Al momento de escribir este artículo, la aplicación KMail tiene varios reportes de inestabilidad. Aunque los pasos siguientes se han probado, es recomendable cambiar a otro cliente de correo como Mozilla Thunderbird o Evolution.
{: .prompt-warning}

KMail es uno de los tantos componentes de <a aria-label="Kontact (opens in a new tab)" href="https://kontact.kde.org" target="_blank">Kontact</a>, un grupo de aplicaciones para la gestión del día a día bajo licencia GPL. KMail está preinstalado en las distribuciones con KDE pero se puede utilizar en cualquier distribución. Pero esta aplicación tiene un problemilla, y es que suele dar errores al intentar conectar tu cuenta de Gmail así que vamos a ponernos manos a la obra para saber como solucionarlo y poder acceder a nuestro correo sin usar el navegador.

## Instalación de KMail

Mejor primero vemos como instalar esta suite. Si usas KDE casi seguro que ya la tienes y puedas saltarte este paso. Si no lo tienes la instalación es sencilla, y recuerda que puede instalarse en cualquier distro. Primero de todo actualizaremos los repositorios y luego instalaremos.

```terminal
sudo apt update
sudo apt install kmail
```

Una vez finalizado ya podremos encontrar KMail en nuestro sistema.

## Configuraciones previas con Gmail

Vamos a preparar el sistema para permitir el acceso de KMail a nuestra cuenta de correo de Google, para ello solo tenemos que entrar en <a href="https://myaccount.google.com/lesssecureapps" target="_blank">este enlace</a> y permitir el acceso a aplicaciones inseguras. Es importante mantener esta opción siempre habilitada o todo se irá al traste.

## Vincular cuenta de Gmail a KMail

Es la hora del momento decisivo. Es muy importante que se haga siguiendo estos pasos y no desde la opción de "<strong>Añadir cuenta...</strong>", esta crea una configuración errónea de la cuenta que lleva a un error. Nos iremos a <strong>Preferencias</strong> y "<strong>Configuración de KMail...</strong>" y en "<strong>Recepción</strong>" le daremos a la opción de "<strong>Añadir cuenta de correo</strong>" dentro de la opción de añadir.

![](/assets/2019/10/asistente-de-cuentas-editado.png)

Escribiremos nuestro nombre, dirección de correo electrónico de Google y contraseña de este. <strong>Importante desmarcar las preferencias del proveedor</strong>.

La siguiente ventana es una opción de seguridad preguntando si se desea usar un par de claves para cifrar los mensajes. Esto también puede usarse para firmar, lo mejor es conseguir alguna firma de una entidad certificadora ya que los certificados autofirmados no tienen mucha validez, en el artículo sobre los <a href="https://odiseageek.es/que-es-el-certificado-ssl/">certificados SSL</a> se habla más de este tema.

En la siguiente ventana se tiene que seleccionar el tipo de cuenta de correo. Los usados siempre suelen ser IMAP y POP3, de los cuales siempre recomiendo el uso del IMAP en cualquier circunstancia. A continuación se tiene que poner el servidor de correo entrante y saliente, donde pondremos lo siguiente.

![](/assets/2019/10/servidores-corre-gmail-kmail.png)

Al darle a siguiente se empezará a configurar la cuenta y se abrirá una ventana de carga que, al ponerla en pantalla completa, se verá que es una página de inicio de sesión de Google solicitada por KDE. Tras poner nuestra contraseña y permitir el acceso se terminará la configuración y podremos acceder a nuestros correos desde la aplicación.

### En caso de error...

Desde hace un tiempo al poner la contraseña en el último paso sale un mensaje de Google diciendo que no ha confirmado la aplicación. En este caso terminaremos la configuración como si todo hubiera ido correctamente. Volveremos a irnos a la <strong>configuración del KMail</strong> y en el apartado de "<strong>Recepción</strong>" seleccionaremos nuestra cuenta y le daremos a "<strong>Modificar</strong>". En la opción de servidor IMAP pondremos "imap.googlemail.com". En el apartado de "<strong>Envío</strong>" haremos lo mismo y pondremos "smtp.googlemail.com".

Con esto KMail ya debería poder acceder a Gmail y empezará a descargar los correos.

![](/assets/2019/10/sincronizacion-kmail-1024x599.png)
_Dependiendo de la cantidad de correos KMail tardará más o menos en terminar la sincronización. Esta cuenta la tengo desde 2013 y la sincronización tardó entre 3 y 4 horas._

## Resultados

De esta forma he comprobado que se ha podido mandar y recibir con éxito correos de Gmail por KMail por lo que todo ha salido bien. A continuación muestro pruebas del envío y la llegada de los correos.

![](/assets/2019/10/correo-admin.png)

![](/assets/2019/10/correo-personal.png)
