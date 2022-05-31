---
layout: post
title: Qué es el DDNS
date: 2019-09-30 09:00:15.000000000 +02:00
type: post
published: true
password: ''
status: publish
categories:
- Servicios en Red
tags:
- cg-nat
- ddns
- dns
---
Si mi memoria no me falla, en 2018 se acabaron todas las direcciones IPv4 públicas que se podían dar, esto significa que tampoco se podía otorgar una misma IP a un único dispositivo, lo que hacían las operadoras de Internet era que, cuando se desconectaba un router, se liberaba su IP pública y se le otorgaba a otro. Esto también aumenta el precio de las direcciones IPv4 públicas y se especula sobre su uso.

## ¿Como afecta esto al DNS?

El DNS se encarga de vincular un FQDN (visualmente es parecido a una URL) con la dirección IP del servidor correspondiente. Esto facilita enórmemente la tarea de aprendernos las direcciones de acceso a nuestros servicios favoritos. Por ejemplo, si quieremos entrar en Wikipedia, nos es más fácil recordar `wikipedia.org` que `91.198.174.192`.

El problema llega cuando la dirección 91.198.174.192 no es fija y la van cambiando cada cierto tiempo, en ese caso nos tendríamos que acordar cada vez de un número distinto y el DNS no serviría porque la IP vinculada a wikipedia.org ahora es posible que lleve a la IP de Google.

Para solucionar esto está el <strong>DDNS o DNS Dinámico</strong>, este servicio actualiza automáticamente la dirección IP vinculada cuando detecta que esta cambia, de forma que mantiene la misma URL sin problemas.

## Usos

Esto es muy efectivo a la hora de ahorrar direcciones IP. Los hostings pueden ir jugando con estas sin que los visitantes de las páginas se percaten de ello. Para uso doméstico es una maravilla. Si tienes un servidor en casa y quieres que que se pueda acceder desde fuera necesitarás un DDNS porque la compañía con la que tengas contratado Internet va a ir cambiando tu IP (a no ser que tenga CG-NAT, en ese caso no podrás usar un DDNS).

## Servicio gratuito

Para tener un DDNS se necesita un hosting que gestione únicamente esto. Puedes tener el servidor en casa y usar <a href="https://www.noip.com" target="_blank">no-ip</a> para tener un ddns con un subdominio algo feo, como *.ddns.net o *.zapto.org. Lo único que se tiene que hacer es confirmar cada 30 días que se sigue usando este servicio haciendo click a un enlace que se mandará por correo electrónico.