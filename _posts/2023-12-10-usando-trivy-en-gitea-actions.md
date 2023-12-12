---
layout: post
title: Usando Trivy en Gitea Actions. Escaneo de vulnerabilidades con herramientas de código abierto.
date: 2023-12-10 21:22:00 +02:00
type: post
password: ''
status: publish
published: true
status: publish
categories:
- Seguridad Informática
tags:
- github
- git
- gitea
- cicd
---

> Esta entrada cuenta con un [TL;DR](#tldr)
{: .prompt-tip }

Voy a explicar como implementar Trivy en el CI/CD de Gitea. Si no sabes muy bien alguno de esos puntos lo explicaré a continuación, si no, puedes bajar directamente al apartado de [implementación](#creando-un-gitea-action).

## Qué es CI/CD

CI/CD viene de las siglas **Continuous Integration/Continuous Delivery** o **Implementación Contínua/Despliegue Contínuo** y viene siendo, a grandes rasgos, automatizar cosas durante el desarrollo de código. Estos son los puntos clave:

- **Automatización de compilación y testeo**: Automáticamente, con cada cambio en el repositorio git, se compila el nuevo código y se realizan pruebas para detectar fallos de forma temprana.
- **Reportes intantáneos**: Si algún test falla, se reporta automáticamente para que se pueda solucionar lo antes posible.
- **Asegurar la calidad del código**: Los entornos de desarrollo pueden tener definidas ciertas políticas, estándares, buenas prácticas... básicamente, unas normas. Puede velar por el cumplimiento de las buenas prácticas de la empresa.
- **Despliegues automáticos en producción**: A cada cambio en el repositorio y tras pasar las pruebas, se puede automatizar el despliegue en los servidores de producción, evitando así tener que hacerlo manualmente.

En resumidas cuentas, gracias al CI/CD se reduce el tiempo de despliegue, el error humano y se flexibilizan y estandarizan los procesos de prueba y despliegue. Igulamente, intentaré dedicar una entrada a parte a explicar la metodología DevOps y DevSecOps, aunque Internet ya esté plagado de ellas.

### Automatizaciones de seguridad por CI/CD

Los CI/CD pueden aprovecharse para automatizar ciertos controles de seguridad como vigilar los principales riesgos de las aplicaciones web con el OWASP Top 10 (SQLi, XSS, pérdidas de control de acceso...), avisar de incumplimientos de las políticas de seguridad de código, dependencias vulnerables, contenedores desactualizados, código malicioso ofuscado y todo lo que se te ocurra. Las posibilidades son enormes. Por ejemplo, abrir una issue si detecta secretos en el último commit, bloquear un pull requests a `master` si una llamada permite una inyección SQL o mandar un correo si hay que actualizar la imagen docker que se usa como base.

## Introduciendo Trivy, el escáner de seguridad de código abierto

![](https://raw.githubusercontent.com/aquasecurity/trivy/main/docs/imgs/logo.png){: w="175" }

Ahora bien, toda esta magia debe hacerse con las herramientas adecuadas. Soy consciente que la mayoría de estas implementaciones solo son viables en entornos empresariales y no en proyectos de prueba que a las 3 semanas quedan en el olvido (a no ser que seas un rarito de la seguridad como yo que gasta más tiempo trasteando con estas cosas que en el propio proyecto).

Una herramienta con la que he tenido buenos resultados, es sencilla y es de código libre es **Trivy**, un escaner de seguridad de código libre creado por [Aqua Security](https://www.aquasec.com/). En esta entrada veremos como integrarlo en un CI/CD para escanear el código de un repositorio git y la última imagen docker creada en base a este, pero también es posible usarlo para detectar vulnerabilidades en dependencias de código o de sistemas, fallos en IaaC, Kubernetes y alguna cosa más. También puede usarse vía CLI para lanzarlo manualmente mientras se escribe código o se compila una imagen en local.

## Introduciendo Gitea Actions, un GitHub ligero y abierto

![](https://about.gitea.com/gitea-text.svg){: w="175" }

Aunque sigo teniendo GitHub para cosas puntuales, desde hace más de un año que llevo usando Gitea como mi servidor git remoto, en él tengo mi propia organización, repositorios propios públicos y privados, mis imágenes docker personalizadas y, por supuesto, mis pipelines.

## Creando un Gitea Action

> El Gitea Actions es compatible con GitHub Actions, si hiciera falta alguna modificación, se avisa en el texto.
{: .prompt-info }

1. Primero vamos a crear el archivo YAML que Gitea detectará automáticamente para ejecutar el workflow, este será en `.gitea/workflows/security.yml`. Si es para GitHub, debe estar en el directorio `.github/workflows/`. 
2. Ahora, en el archivo, definimos el nombre de la acción y cuando se ejecutará, en este ejercicio haremos que se lance todos los días 15 a las 4AM.

    ```yaml
    name: Security Scan

    on:
    schedule:
        - cron: "0 4 15 * *"
    ```

3. Añadimos el espacio de trabajo desde el que se ejecutará todo:

    ```yaml
    jobs:
    scan:
        runs-on: ubuntu-latest
        container:
        image: catthehacker/ubuntu:act-latest # Ubuntu image compatible with nektos/act, the gitea action runner
    ```
4. Dentro, añadimos un punto para obtener el último commit y otro para sacar el nombre del repositorio:
    ```yaml
        steps:
        - name: Checkout
            uses: actions/checkout@v3
            with:
            fetch-depth: 0
        - name: Get Meta
            id: meta
            run: |
            echo REPO_NAME=$(echo ${GITHUB_REPOSITORY} | awk -F"/" '{print $2}') >> $GITHUB_OUTPUT        
    ```
5. Ahora hacemos el escaneo de la última imagen usando el paso de trivy. Como parámetros le indicaremos que nos muestre únicamente las vulnerabilidades corregidas, en una tabla, que nos vuelque todo a un archivo, y que el código de error sea siempre 0.
    ```yaml
        - name: Image Scan
            uses: aquasecurity/trivy-action@master
            with:
            image-ref: 'gitea.host.tld/user-or-org-name/{% raw %}${{ steps.meta.outputs.REPO_NAME }}{% endraw %}:latest'
            format: 'table'
            exit-code: '0'
            ignore-unfixed: true
            vuln-type: 'os,library'
            output: 'image-scan.txt'
    ```
6. Hacemos lo mismo con el escaneo en el propio repositorio. Como ya lo hemos descargado en el paso 4, usamos la opción `fs` (filesystem).
    ```yaml
        - name: Code Scan
            uses: aquasecurity/trivy-action@master
            with:
            scan-type: 'fs'
            ignore-unfixed: true
            format: 'table'
            output: 'code-scan.txt'
            exit-code: '0'
    ```
7. Este último paso sí que difiere. En GitHub existe un paso creado por la comunidad para subir issues al proyecto. Como en Gitea no es posible, usaremos la API para publicar el contenido de ambos ficheros.
    ```yaml
        - name: Create Issue
            env:
            API_TOKEN: {% raw %}${{ secrets.ORG_ISSUES_TOKEN }}{% endraw %}
            run: |
            ISSUE_TITLE="Security report $(date +'%d/%m/%Y')"
            ISSUE_CONTENT=$(cat 'image-scan.txt' 'code-scan.txt')
            curl -X POST -H "Authorization: token ${API_TOKEN}" -H "Content-Type: application/json" \
                -d "{\"title\": \"$ISSUE_TITLE\", \"body\": \"$ISSUE_CONTENT\"}" \
                https://gitea.host.tld/api/v1/repos/user-or-org-name/{% raw %}${{ steps.meta.outputs.REPO_NAME }}{% endraw %}/issues
    ```

Podemos guardar el archivo, pero no hemos terminado. En el último paso, podemos ver que hemos definido la variable `API_TOKEN` y hace referencia a `{% raw %}${{ secrets.ORG_ISSUES_TOKEN }}{% endraw %}`. Este es un secreto que solo el CI puede consultar. Pero claro, primero debemos crearlo.

### Definiendo el secreto

El secreto va a ser un API Token para publicar issues. Siguiendo la política de privilegio mínimo, vamos a crear uno únicamente con este fin. Accedemos a la configuración del usuario y en **Aplicaciones** podremos crear el token de escritura en las issues.

![](/assets/2023/12/gitea-token-creation.png)

Ahora toca copiar el token resultante y definirlo como secreto en los **Ajustes de usuario** > **Acciones** > **Secretos**. Para esta ocasión, y como el token se va a usar en otros proyectos, lo definiré a nivel de organización desde la **Configuración de la organización** > **Acciones** > **Secretos**.

![](/assets/2023/12/gitea-secret-creation.png)

Como el proyecto desde el que se ejecuta el workflow es parte de la organización, el secreto se puede consultar durante la ejecución.

### Comprobación

Cuando llegue el momento, el CI ejecutará el workflow y lanzará el escaneo de seguridad como hemos definido.

![](/assets/2023/12/security-scan-ci.png)

Y veremos como el usuario del API Token ha creado un issue con una tabla bastante amorfa (pero entendible) con los resultados.

![](/assets/2023/12/gitea-ci-issue-creation.png)

Como punto de mejora, podemos hacer que Trivy nos devuelva los resultados en formato JSON y, luego, nosotros lo parseamos a una tabla Markdown más aseada o en el formato que deseemos.

## TL;DR

1. Crear un secreto con el API Token para crear issues en el proyecto. Llamarlo `ORG_ISSUES_TOKEN` y guardarlo en el proyecto o la organización.
2. Usamos este código como workflow:
    ````yaml
    name: Security Scan

    on:
    schedule:
        - cron: "0 4 15 * *"

    jobs:
    scan:
        runs-on: ubuntu-latest
        container:
        image: catthehacker/ubuntu:act-latest # Ubuntu image compatible with nektos/act, the gitea action runner
        steps:
        - name: Checkout
            uses: actions/checkout@v3
            with:
            fetch-depth: 0
        - name: Get Meta
            id: meta
            run: |
            echo REPO_NAME=$(echo ${GITHUB_REPOSITORY} | awk -F"/" '{print $2}') >> $GITHUB_OUTPUT
            echo REPO_VERSION=$(git describe --tags --always | sed 's/^v//') >> $GITHUB_OUTPUT            
        - name: Image Scan
            uses: aquasecurity/trivy-action@master
            with:
            image-ref: 'gitea.host.tld/user-or-org-name/{% raw %}${{ steps.meta.outputs.REPO_NAME }}{% endraw %}:latest'
            format: 'table'
            exit-code: '0'
            ignore-unfixed: true
            vuln-type: 'os,library'
            output: 'image-scan.txt'
        - name: Code Scan
            uses: aquasecurity/trivy-action@master
            with:
            scan-type: 'fs'
            ignore-unfixed: true
            format: 'table'
            output: 'code-scan.txt'
            exit-code: '0'
        - name: Create Issue
            env:
            API_TOKEN: {% raw %}${{ secrets.ORG_ISSUES_TOKEN }}{% endraw %}
            run: |
            ISSUE_TITLE="Security report $(date +'%d/%m/%Y')"
            ISSUE_CONTENT=$(cat 'image-scan.txt' 'code-scan.txt')
            curl -X POST -H "Authorization: token ${API_TOKEN}" -H "Content-Type: application/json" \
                -d "{\"title\": \"$ISSUE_TITLE\", \"body\": \"$ISSUE_CONTENT\"}" \
                https://gitea.host.tld/api/v1/repos/user-or-org-name/{% raw %}${{ steps.meta.outputs.REPO_NAME }}{% endraw %}/issues    
    ````