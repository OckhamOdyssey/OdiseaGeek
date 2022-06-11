---
layout: post
title: Vincular calendario de Office 365 a Google Calendar con Microsoft Power Automate
date: 2019-12-23 14:28:19.000000000 +01:00
type: post
parent_id: '0'
published: true
password: ''
status: publish
categories:
- Misceláneo
tags:
- calendar
- google
- microsoft
- office 365
---
![](/assets/2019/12/all_office_365.png)
_Lista de todas las aplicaciones online que incluye._

Seguro que no soy el único que tiene cuentas de distintas compañías como Google y Microsoft. En mi caso utilizo la primera para uso personal y la segunda es la que me ha ofrecido una institución de voluntariado de la que soy muy activo. El calendario que uso es solo uno, para que no se solapen los eventos, pero desde hace poco el equipo al que pertenezco está usando el calendario de Outlook para coordinarse así que se me ha ocurrido la idea de vincularlos automáticamente. <strong>Aviso que usamos la edición empresarial de Office 365, no se como funcionan las otras ediciones ni si tienen todos estos complementos incluidos.</strong>

## Conozcamos las herramientas

Office 365 es un paquete de herramientas y ofimática creado por Microsoft. En este se incluyen los programas como Excel, PowerPoint, Word, Outlook, etcétera. En mi caso también incluye OneDrive, OneNote, Yammer, Forms, Planner y, la que nos interesa ahora mismo, Power Automate. Esta potentísima herramienta permite automatizar tareas y darle instrucciones al resto del Office 365 para que funcione de forma más inteligente. Existen plantillas para facilitarnos la tarea.

## Cómo va a funcionar

El "flujo" (así lo llama Microsoft) se mantendrá a la espera eternamente, su mecha de ignición será la creación o aceptación de un evento en el calendario de Outlook. En ese momento, el flujo se pondrá en contacto con nuestra cuenta de Google y creará el mismo evento en este. Parece fácil pero el sistema de Google Calendar ha cambiado hace un tiempo por lo que las plantillas que aparecen en Microsoft están desfasadas, y ya no hay soporte para este tipo de herramientas.  De manera que no usaremos plantilla y tendremos que escribir dos líneas de código para arreglar lo que Microsoft no quiere arreglar.

## Creación del evento

Dentro de Power Automate accederemos a la pestaña de Crear y seleccionaremos la opción de flujo automatizado. Aquí pondremos el nombre que queramos y el desencadenante, el cual será la creación de un evento en Outlook.

![](/assets/2019/12/Screenshot_7-1024x552.png)

Al darle a siguiente entraremos en el panel del lujo, seleccionaremos el calendario de Outlook donde creamos los eventos, usualmente se llama simplemente "Calendario". Ahora añadimos desde el botón "Nuevo paso" lo que queremos que ocurra tras crear el desencadenante.

Al buscar la palabra "Google" nos aparecerá ya la opción de crear un evento en Google Calendar.

![](/assets/2019/12/Screenshot_8.png)

Seleccionaremos nuestra cuenta de Google (es posible que antes nos haya pedido iniciar sesión para poder dar permisos a Power Automate).

A continuación hay que indicar la hora de inicio y de finalización del evento. Si ponemos que utilice la hora de Outlook en el calendario nos dará error al ejecutar el flujo debido a que Google y Microsoft no utilizan el mismo sistema para marcar las horas. Para ello tenemos que escribir un código que haga la conversión.

Para la hora de inicio escribimos el siguiente código:

```
convertToUtc(triggerOutputs()?['body/start'], 'GMT Standard Time')
```

Para la hora de finalización escribimos el siguiente código:

```
convertToUtc(triggerOutputs()?['body/end'], 'GMT Standard Time')
```

Para todo lo demás podemos personalizarlo como queramos. Recomiendo marcar que el título y la descripción sean la misma. Una vez terminan el flujo se debe hacer una comprobación creando un evento en Outlook e iniciar el flujo. A partir de ahora todos los eventos que se creen o que se acepten en Outlook se añadirán a Google Calendar automáticamente.

![](/assets/2019/12/Screenshot_9-1024x261.png)