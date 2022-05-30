---
layout: post
title: Qué es el certificado SSL
date: 2019-09-28 15:36:37.000000000 +02:00
type: post
parent_id: '0'
published: true
password: ''
status: publish
categories:
- Seguridad Informática
tags:
- certificado digital
- comodo
- fnmt
- godaddy
- lets encrypt
- ssl
--->Mantener la seguridad es primordial en Internet, por ello los certificados SSL ya son prácticamente obligatorios en las páginas web, especialmente en las páginas de inicio de sesión y compras por Internet.

El certificado SSL (<strong>S</strong>ecure <strong>S</strong>ockets <strong>L</strong>ayer) tiene una doble función:
<ul>
<li><strong>Seña de identidad</strong>: Como una firma o un notario, el certificado SSL asegura que la página que se está visualizando es la que realmente dice ser. </li>
<li><strong>Cifrado</strong>: El certificado SSL también es usado como un método de cifrado, haciendo que los datos que viajan lo hagan de forma segura evitando que los ciberdelincuentes roben datos sensibles.</li>
</ul>

## Cómo funciona

Los certificados SSL se dividen en dos partes, la clave privada y la clave pública. La clave pública se manda a los usuarios que han accedido a la página y utilizan esta para cifrar los datos que se envían. El receptor, el servidor web, utiliza su clave privada para descifrar los datos. Hay que remarcar que solo con la clave pública se puede cifrar y solo con la clave privada se puede descifrar, esto es lo que se llama <a href="https://es.wikipedia.org/wiki/Criptografía_asimétrica">criptografía asimétrica</a>. De esta forma las comunicaciones entre cliente y servidor son seguras, por lo que se puede estar seguro al comprar en una página web con este tipo de certificados.

Puedes generar tu mismo un par de claves para mantener tu sitio web seguro, lo que se llama <strong>certificado autofirmado</strong>. Existen aplicaciones y servicios que se encargan de esto como GPG, que se puede encontrar en los repositorios de Ubuntu. El problema de esto es que no hay nada que confirme que ese sitio web sea tuyo por lo que saldrá una advertencia cada vez que alguien entre a este sitio.

![Mensaje de advertencia de Ópera al intentar entrar en un sitio web con un certificado autofirmado.](/assets/2019/09/autofirmado-opera-1024x480.png)

## Entidades certificadoras
Existe el riesgo de suplantación de una página web, donde un atacante simula tu página y, al igual que tú, le añade un certificado autofirmado, en este caso no hay forma de saber cual es la verdadera página y por eso existen las <strong>entidades certificadoras</strong> conocidas por sus siglas en inglés <strong>CA</strong>.

Estas entidades son responsables de emitir certificados de confianza, se encargan de comprobar y asegurar que el particular o la empresa que solicita el certificado es quien realmente es y que este se instale en la web correcta. Una vez han confirmado todo otorgan el certificado, dando su confianza de que esa página es la que dice ser. En caso de un intento de suplantación, el atacante no podrá certificar que eres tú, por lo que ninguna CA le dará su aprobación y un certificado autofirmado no da credibilidad.

Estos certificados pueden aportar la misma encriptación que un autofirmado pero hará que el navegador añada un candado verde a la URI de la página dándole confianza y haciendo que no salgan mensajes de advertencia.

Existen muchas entidades certificadoras, siendo <a aria-label="COMODO (abre en una nueva pestaña)" href="https://www.comodoca.com/ssl-certificate-comparison" target="_blank">COMODO</a> y <a aria-label="GoDaddy (abre en una nueva pestaña)" href="https://es.godaddy.com/web-security/ssl-certificate" target="_blank">GoDaddy</a> los que más he visto por la red. Como cualquier empresa, compiten entre ellos y ofrecen distintos precios, características e incluso garantías en caso de sufrir un ataque exitoso.

## Tipos de certificados

Existen tres tipos de certificados digitales. Estos se diferencian por la amplitud de seguridad que ofrecen.

<ul>
<li><strong>Certificado SSL único</strong>. Este es muy común, certifica únicamente una página o un único dominio.</li>
<li><strong>Certificado SSL Wildcard</strong>. Actualmente es el que rige en este blog. Es un certificado que implica al dominio y a todos los subdominios.</li>
<li><strong>Certificado SSL multi-dominio</strong>. Sinceramente no creo haber visto nuca este tipo de certificado. Tal y como indica su nombre certifica varios dominios que no tienen nada en común. </li>
</ul>

Luego, dependiendo de la CA que se escoja y los servicios que ofrezca, los bits de cifrado pueden ser mayores (ofreciendo así mayor seguridad) o incluir el nombre de la empresa en el título.

## Certificados gratuitos. Let's Encrypt y FNMT

A parte de las empresas existen entidades que ofrecen certificados de forma gratuita y sí, son fiables.

<img class="alignright" "wp-image-2103"="" style="width: 150px;" src="https://odiseageek.es/wp-content/uploads/2019/09/letsencrypt-logo.png" alt="">Por un lado está <strong><a aria-label="Let's Encrypt (abre en una nueva pestaña)" href="https://letsencrypt.org/es/" target="_blank">Let's Encrypt</a></strong>, esta CA es creada por una organización sin ánimo de lucro que busca un Internet sin barreras y una mayor seguridad. Es una CA muy popular y que una gran cantidad de hostings (<a href="https://odiseageek.es/creando-el-blog/" target="_blank" rel="noopener noreferrer">incluido el mío</a>) incluyen herramientas para la solicitud, instalación y renovación de sus certificados de forma automática, si el hosting no incluye esta herramienta también se puede instalar certbot en el servidor y dejar que haga todo el trabajo. Siempre veo muchas webs con estos certificados, cada vez más, y las mías también lo tienen. Desde 2018 también otorga certificados Wildcard.

<img class="alignright" "wp-image-2112"="" style="width: 150px;" src="https://odiseageek.es/wp-content/uploads/2019/09/fnmt-logo.png" alt="">Luego está la <a aria-label="FNMT (abre en una nueva pestaña)" href="http://www.fnmt.es/ceres" target="_blank">FNMT</a> o Fábrica Nacional de Moneda y Timbre, una entidad pública española que es más conocida por la serie <em>La Casa de Papel</em> que por otra cosa. Y, a pesar de lo que diga la serie, no se encarga de imprimir dinero. Uno de los servicios públicos que incluye es la cesión de certificados digitales a los ciudadanos. Estos sirven para firmar documentos electrónicos, acceder a la declaración de la renta y, por qué no, la firma de páginas web. Nunca he visto que se haga esto último con un certificado de la FNMT y supongo que será por algo. Se puede solicitar por Internet siguiendo unos requisitos <a aria-label="un tanto quisquillosos (abre en una nueva pestaña)" href="https://www.sede.fnmt.gob.es/certificados/persona-fisica/obtener-certificado-software/solicitar-certificado" target="_blank">un tanto quisquillosos</a>.