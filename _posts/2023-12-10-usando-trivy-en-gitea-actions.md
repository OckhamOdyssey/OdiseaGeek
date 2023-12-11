---
layout: post
title: Usando Trivy en Gitea Actions. Escaneo de vulnerabilidades con herramientas de código abierto.
date: 2023-12-10 21:00:00 +02:00
type: post
password: ''
status: publish
published: true
status: publish
categories:
- Seguridad Informática
tags:
- cloudflare
- http
- headers
---
En esta entrada voy a explicar como implementar Trivy en el CI/CD de Gitea. Si no sabes muy bien alguno de esos puntos lo explicaré a continuación, si no, puedes bajar directamente al apartado de implementación.

# Qué es CI/CD

CI/CD viene de las siglas **Continuous Integration/Continuous Delivery** o **Implementación Contínua/Despliegue Contínuo** y viene siendo, a grandes rasgos, automatizar cosas durante el desarrollo de código. Estos son los puntos clave:

- **Automatización de compilación y testeo**: Automáticamente, con cada cambio en el repositorio git, se compila el nuevo código y se realizan pruebas para detectar fallos de forma temprana.
- **Reportes intantáneos**: Si algún test falla, se reporta automáticamente para que se pueda solucionar lo antes posible.
- **Asegurar la calidad del código**: Los entornos de desarrollo pueden tener definidas ciertas políticas, estándares, buenas prácticas... básicamente, unas normas. Puede velar por el cumplimiento de las buenas prácticas de la empresa.
- **Despliegues automáticos en producción**: A cada cambio en el repositorio y tras pasar las pruebas, se puede automatizar el despliegue en los servidores de producción, evitando así tener que hacerlo manualmente.

En resumidas cuentas, gracias al CI/CD se reduce el tiempo de despliegue, el error humano y se flexibilizan y estandarizan los procesos de prueba y despliegue.

## Automatizaciones de seguridad por CI/CD

Los CI/CD pueden aprovecharse para automatizar ciertos controles de seguridad como vigilar los principales riesgos de las aplicaciones web con el OWASP Top 10 (SQLi, XSS, pérdidas de control de acceso...), avisar de incumplimientos de las políticas de seguridad de código, dependencias vulnerables, contenedores desactualizados, código malicioso ofuscado y todo lo que se te ocurra. Las posibilidades son enormes. Por ejemplo, abrir una issue si detecta secretos en el último commit, bloquear un pull requests a `master` si una llamada permite una inyección SQL o mandar un correo si hay que actualizar la imagen docker que se usa como base.

# Introduciendo Trivy, el escáner de seguridad de código abierto

Ahora bien, toda esta magia debe hacerse con las herramientas adecuadas. Soy consciente que la mayoría de estas implementaciones solo son viables en entornos empresariales y no en proyectos de prueba que a las 3 semanas quedan en el olvido (a no ser que seas un rarito de la seguridad como yo que gasta más tiempo trasteando con estas cosas que en el propio proyecto).

Una herramienta con la que he tenido buenos resultados, es sencilla y es de código libre es **Trivy**, un escaner de seguridad de código libre creado por [Aqua Security](https://www.aquasec.com/). En esta entrada veremos como integrarlo en un CI/CD para escanear el código de un repositorio git y la última imagen docker creada en base a este, pero también es posible usarlo para detectar fallos en IaaC, Kubernetes y alguna cosa más. También puede usarse vía CLI para lanzarlo manualmente mientras se escribe código o se compila una imagen en local.