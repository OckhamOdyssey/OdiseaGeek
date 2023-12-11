---
title: Usando Trivy en Gitea Actions. Escaneo de vulnerabilidades con herramientas de código abierto.
date: 2023-12-10 21:00:00 +02:00
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

# Automatizaciones de seguridad por CI/CD

