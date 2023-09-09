---
layout: post
title: Creando el blog
date: 2019-09-21 07:01:05.000000000 +02:00
type: post
parent_id: '0'
published: true
password: ''
status: publish
categories:
- Misceláneo
tags:
- let's encrypt
- siteground
- ssl
- wordpress
---
Que mejor forma de empezar que mostrando mis vivencias al crear mi primera página web con hosting y con Wordpress.

Desde hacía un tiempo que tenía algunos hostings en la mira, pero solo para recomendar a un amigo. Después, tras varios días buscando recomendaciones y comparando servicios y ofertas **me decanté por Sitegrounds**. Con lo desconfiado que soy buscaba ir a lo seguro, nada de chollos ni anuncios por Internet y esto lo recomendaban en todos lados y había visto otras páginas web con este hosting.

## ¿Por qué me decanté por Sitegrounds?

Principalmente por lo que ya he comentado. he visto que páginas importantes este sitio y que <a href="https://wordpress.org/hosting/">lo recomiende el propio Wordpress</a> es más que suficiente. A demás, me aseguré que el hosting permitiera el uso de <a href="https://letsencrypt.org/es/">Let's Encrypt</a> como medida SSL, y es que tiene hasta su propio instalador.<br />Cuesta aproximadamente 4'77€ al mes (3'95€ que dice la página web más IVA) e incluye el dominio por un año, cuentas de correo ilimitadas, subdominios ilimitados, transferencia mensual ilimitada y 10GB de espacio en el disco.

## Problema con los .es

Hace meses ayudé a un amigo con la compra de un dominio usando otro hosting, <a href="https://www.ionos.es">IONOS</a> para ser exactos. Cuando compró el dominio yo estaba delante para verlo y en un minuto ya estaba registrado en who.is. Entonces me preguntaba ¿Por qué el mío no está? ¿Por qué han pasado dos horas y sigo sin poder acceder?. La respuesta está en la <a href="https://www.dominios.es">administración española</a>. Esta te hace pasar por unos trámites (en este caso a la empresa de hosting) antes de empezar con la difusión DNS. Esto yo no lo sabía, así que he estado dos días mordiéndome las uñas esperando que no sea un fallo y que sea algo normal. Hasta contacté con el servicio técnico.

Durante ese tiempo intentaba entrar al servidor directamente mediante a IP del servidor, pero claro, estaba configurado para ser usado mediante el DNS y habían enlaces y herramientas que no me funcionaban. Lo solucioné cambiando la configuración de la red de mi ordenador forzando las peticiones DNS a los servidores de Siteground.

![](/assets/2019/09/DNS1.png)

De esta forma se solicita primero las peticiones DNS al servidor de Siteground, por lo que podría ver mi página web, y luego al DNS de Google, que me garantiza acceso al resto de Internet.

El día 19 de septiembre de 2019 fue cuando vi por primera vez el blog sin tener que forzar nada, estaba todo listo y solté un buen suspiro de tranquilidad.

## Construcción

Era mi primera vez en Wordpress así que todo lo de buscar una plantilla, editarla y eso no era algo a lo que estaba acostumbrado. Por suerte, el funcionamiento de las entradas y las categorías me parecen muy similar a las de Joomla!, por lo que solo tengo que recordar ese horrible mes en el que estuve haciendo una página con ese CMS para un trabajo de clase y todo irá genial.

La página todavía está muy verde y le falta mucho diseño (ni si quiera tiene aún favicon) pero la experiencia me ha dicho que eso no se debe hacer ahora, mejor poco a poco mientras la página va creciendo.

Mientras tanto le he puesto un <a href="https://odiseageek.es/que-es-el-certificado-ssl/">certificado SSL Wildcard</a> de Let's Encrypt, como en mi servidor Nextcloud, y unos pocos vectores SVG, sacados de una biblioteca libre y de la <a href="https://commons.wikimedia.org/wiki/Breeze_icons">galería de iconos</a> del tema por defecto de KDE Plasma, Breeze. Estos iconos cuentan todos con <a href="https://www.gnu.org/licenses/licenses.html#GPL">licencia GNU</a>.

También soy un desastre con las combinaciones de colores así que he usado una herramienta llamada <a href="https://coolors.co">Coolors</a> para buscar alguna combinación que me guste.

Buscaré más adelante tutoriales sobre Wordpress, que se que tiene mucho más potencial. También para las cookies analíticas, plugins, SEO, etc.
