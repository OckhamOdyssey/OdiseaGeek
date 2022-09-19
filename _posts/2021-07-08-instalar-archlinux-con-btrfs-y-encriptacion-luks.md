---
layout: post
title: Instalar ArchLinux con BTRFS y encriptación LUKS
date: 2021-07-08 20:49:18.000000000 +02:00
type: post
parent_id: '0'
published: true
password: ''
status: publish
categories:
- Apuntes ASIR
- Implantación de Sistemas Operativos
- Misceláneo
tags:
- Arch Linux
- btrfs
- encriptación
- grub
- linux
- luks
---

Tras varios meses usando Manjaro como sistema principal, viendo la comodidad que me aporta y los pocos problemas que he tenido (comparado con los que me dio Ubuntu), he decidido dar el paso de migrar a Arch Linux. Pero he decidido hacerlo a lo grande, y me veo ya capaz de<strong> instalar Arch Linux con el sistema BTRFS y encriptación LUKS en dos particiones distintas</strong>. No es moco de pavo, lo sé, por eso antes de aventurarme a lo loco he decidido documentarme, hacer una planificación y probar en un equipo externo. Tras lograrlo, he documentado los pasos por secciones. Es probable que no todo se deba realizar al pie de la letra para funcionar en otros equipos, es cuestión de entender las cosas y adaptarlas a las necesidades de cada uno. Dicho esto, empezamos.

## Descarga e instalación del archivo ISO

Como siempre, debemos acceder a la página oficial de Arch Linux para descargar el archivo ISO actualizado. A diferencia de los sistemas operativos por versiones, las ISO desactualizadas no funcionarán. Para ello, habrá que acudir a la <a href="https://archlinux.org/download/" target="_blank">sección de descargas</a> y navegar hasta el apartado <strong>HTTP Direct Downloads</strong>, donde buscaremos nuestro país o uno cercano y descargaremos la ISO desde allí. Si se vive en España recomiendo descargarlo desde el repositorio <strong>rediris</strong> o <strong>cloroformo</strong>, pero tengo constancia que en México pueden haber errores y es mejor descargar la versión de Estados Unidos.

Ahora hay que quemar la imagen ISO en un USB vacío que tengamos. Para ello podemos usar el programa Rufus si trabajamos desde Windows o BalenaEtcher si estamos en GNU/Linux. Hay un poco más de información en<a href="https://www.odiseageek.es/instalar-un-sistema-operativo-a-la-raspberry/#Como_instalar_el_sistema_operativo"> la siguiente entrada</a>.

## Ajustar el idioma del teclado

Antes de ponernos a tocar nada, debemos consultar unos datos y realizar algunas planificaciones, para ello tendremos que escribir algunos comandos. Por defecto, el teclado de Arch Linux está en inglés, así que cambiaremos el idioma a español para facilitarnos las cosas.

```terminal
loadkeys es
```

## Planificar las particiones

Lo primero que tenemos que hacer es saber cuantos discos tenemos, cómo se llaman y si estamos usando una BIOS en modo UEFI o Legacy.

```terminal
lsblk
```

Y, para este ejercicio, supondremos que tenemos un disco sda con 120GB y un sdb de 1TB. El primero, sabiendo que es un SSD, lo usaremos para instalar el sistema operativo, el segundo será la partición `/home`. Ambas encriptadas.

Ahora aparece un problema, si el directorio raíz está encriptado es posible que tengamos problemas a la hora de iniciar el sistema, así que tendremos que separar el o los directorios de arranque para que no se encripten.

Para saber qué directorios de arranque tendremos que tener, hay que saber si nuestro ordenador funciona con [BIOS o UEFI]({% post_url 2020-04-10-el-arranque-del-sistema-medidas-de-seguridad-y-el-grub %}).

```terminal
ls /sys/firmware/efi/efivars
```

Si este comando nos da error, significa que estamos en BIOS/Legacy, si no devuelve error estamos en UEFI.

Si estamos en <strong>BIOS</strong>, todas nuestras particiones deben ser MBR, también llamado DOS. Este tipo de particionado solo permite un total de 4 particiones, que se pueden dividir entre primarias y lógicas. Pero todo eso se especificará en otra entrada.

<p>Si estamos en <strong>UEFI</strong>, las particiones pueden ser tanto MBR como GPT, aunque es más recomendable la segunda, ya que da menos problemas y permite particiones casi ilimitadas, sin diferenciar entre primarias y secundarias.

En nuestro caso estamos en modo UEFI, por lo que a partir de ahora nos centraremos en hacer las particiones para este sistema, todo lo demás puede hacerse igual. Esta es la planificación de nuestro particionado.</p>

| **Disco y partición** | **Tamaño** | **Tipo**         | **Formato** |
|-----------------------|------------|------------------|-------------|
| `sda1`                | 260MiB     | EFI System       | FAT32       |
| `sda2`                | 512MiB     | Linux filesystem | EXT4        |
| `sda3`                | Todo       | Linux filesystem | LUKS        |
| `sdb1`                | Todo       | Linux filesystem | LUKS        |

## Particionado de discos

Usaremos la herramienta fdisk para modificar el primer disco

```terminal
fdisk /dev/sda
```

Seleccionaremos la opción <strong>g</strong> para usar una tabla de partición GPT, luego con la <strong>n</strong> crearemos la primera partición.

![](/assets/2021/07/PyTamK7.png)

La primera opción es el número de la partición, nos aparece el 1 como opción por defecto así que damos a Enter. Luego nos indica cual será el primer sector y le damos a Enter para seleccionar también la opción por defecto. Después, nos pregunta cuanto medirá la partición, aquí tenemos que poner siempre un + seguido del número y M o G. En este caso seleccionamos 260MiB.

Ahora, con la opción <strong>t</strong> editaremos el tipo de partición, si tenemos más de una nos preguntará cual queremos editar. Si no sabemos el código del tipo de partición que queremos podemos pulsar <strong>L</strong> para ver la lista de posibilidades. En nuestro caso queremos una partición EFI, cuyo código es el <strong>1</strong>. Para las otras tres particiones el tipo de particionado no hace falta modificarlos.

Realizamos los mismos pasos para la partición 2 y 3 del disco sda y la única partición de sdb.

## Formateo de particiones

Una vez tenemos las cuatro particiones es hora de darles el formato que queramos. Para ellos tenemos la herramienta <strong>mkfs</strong>, abreviación en inglés de "Dar sistema de formato". Como podemos ver en la tabla de arriba la primera y segunda partición son FAT32 y EXT4 respectivamente, simple. Las otras dos particiones aparece como formato LUKS, esto en realidad no es un formato de por sí, sino el método de encriptación, y se hace con el comando <strong>cryptsetup</strong>. 

Primero vamos a dar un formato FAT32 a la partición EFI, para eso ejecutamos este comando.

```terminal
mkfs.fat -F32 /dev/sda1
```

Ahora daremos formato EXT4 a la partición boot. Con la opción <strong>-L</strong> añadimos lo que se llama labbel, que viene siendo una etiqueta, no es más que algo orientativo.

```terminal
mkfs.ext4 -L boot /dev/sda2
```

Ahora solo nos quedan las dos particiones encriptadas. Para esto, primero debemos cargar en el kernel los módulos de encriptación. Son solo dos comandos.

```terminal
modprobe dm-crypt
modprobe dm-mod
```

Tras esto, ciframos con LUKS las particiones sda3 y sdb1. Para confirmar, nos pedirá escribir YES en mayúsculas y luego la contraseña con la que vamos a encriptar los discos.

```terminal
cryptsetup luksFormat -v -s 512 -h sha512 /dev/sda3
cryptsetup luksFormat -v -s 512 -h sha512 /dev/sdb1
```

## Desbloquear particiones encriptadas

Debemos tener en cuenta que para poder utilizar las particiones encriptadas debemos desbloquearlas primero, eso hará que tengamos por un lado las particiones encriptadas "sdX", y las particiones desencriptadas en mapper. Un ejemplo hará que lo entendamos mejor.

Una partición encriptada se almacena en /dev/sda3, cuando la desencriptemos esta seguirá existiendo pero el acceso desencriptado se encontrará en /dev/mapper/foo, siendo foo una palabra clave que nosotros elegiremos. Para no complicarnos la vida haremos que la palabra clave de la partición sda3 sea "root" y la de sdb1 sea "home".

```terminal
cryptsetup open /dev/sda3 root
cryptsetup open /dev/sdb1 home
```

Tras poner un comando nos solicitará la contraseña de desencriptado que anteriormente le hemos puesto.

Ahora podremos por fin formatear las particiones ahora desencriptadas, volveremos a usar la herramienta <strong>mkfs</strong> pero en las particiones mapper.

```terminal
mkfs.btrfs -L root /dev/mapper/root
mkfs.btrfs -L home /dev/mapper/home
```

## Subvolúmenes BTRFS

A parte de el soporte nativo de comprimirse a si mismo o la posibilidad de montarse mediante un <a href="https://www.odiseageek.es/que-es-un-raid/">RAID</a>, el sistema de ficheros BTRFS también permite crear subvolúmenes. Un subvolumen es un árbol de archivos independiente en un sistema de ficheros que se monta dentro de directorio y puede poseer propiedades independientes. Para no petarnos la cabeza diremos que es una partición dentro de otra partición, no es realmente eso pero aquí nos servirá.

Los subvolúmenes facilitan bastante la administración de los discos, por lo que vamos a utilizarlos en nuestro nuevo sistema. Para ello crearemos dos subvolúmenes dentro del mapper root, uno para el propio directorio raiz y otro como partición de las copias de seguridad del sistema. El mapper home tendrá también un subvolumen propio.

Para ello primero debemos montar de forma temporal el mapper root, indicando que es una partición btrfs, y acceder al propio directorio.

```terminal
mount -t btrfs /dev/mapper/root /mnt; cd /mnt
```

Ahora podemos crear los dos subvolúmenes.

```terminal
btrfs subvolume create root
btrfs subvolume create snapshots
```

Salimos del directorio, desmontamos la partición, montamos la home y creamos su subvolumen

```terminal
cd /
umount /mnt
mount -t btrfs /dev/mapper/home /mnt
cd /mnt
btrfs subvolume create home
cd /
umount /mnt
```

## Montaje de las particiones

Ahora que hemos preparado todas las particiones y los subvolúmenes toca montarlos poco a poco. Hay que tener en cuenta que algunos puntos de montaje están dentro de otros, por lo que habrá que crear primero los directorios de montaje. Este es el orden que hay que seguir para montar las unidades.

Primero montamos el subvolumen root en el que será el directorio raiz, ahora /mnt.

```terminal
mount -t btrfs -o subvol=root /dev/mapper/root /mnt
```

Creamos los directorios para el arranque del sistema.

```terminal
mkdir /mnt/boot
mkdir /mnt/efi
```

Montamos los directorios teniendo en cuenta la tabla del principio.

```terminal
mount /dev/sda2 /mnt/boot
```

```terminal
mount /dev/sda1 /mnt/efi
```

Creamos y montamos el resto de particiones.

```terminal
mkdir /mnt/snapshots
mkdir /mnt/home
mount -t btrfs -o subvol=snapshots /dev/mapper/root /mnt/snapshots
```

## Instalación de paquetes básicos

Ya tenemos todos los discos particionados, formateados y en su sitio, es hora del plato principal: instalar Arch junto con los paquetes base para su funcionamiento. El procedimiento es más simple de lo que parece, se hace con un solo comando pero puede tardar algo de tiempo dependiendo del ordenador que tengamos.

En este punto es importante añadir todos los paquetes que queramos instalar, esto incluye controladores, editores, gestores de red, Wi-Fi... Si uno se olvida siempre puede instalarlo más adelante pero puede ser más complicado.

```terminal
pacstrap -i /mnt base base-devel linux linux-firmware efibootmgr grub nano btrfs-progs
```

Con todo ya instalado, se habrá creado todo el árbol de directorios de un sistema GNU/Linux dentro de /mnt, pero el fstab estará sin modificar o vacío, por lo que tendremos que crearlo. Por suerte, contamos una herramienta para ayudarnos con ello, un comando que muestra toda la configuración de los puntos de montaje.

```terminal
genfstab -U /mnt > /mnt/etc/fstab
```

Aún no tenemos nuestro sistema preparado para usar, pero sí está listo para saltar a él y empezar con las configuraciones iniciales.

## Configuraciones básicas

Estos pasos no os los debéis saltar, son totalmente necesarios para que el sistema arranque siquiera. Primero "saltaremos" al nuevo sistema para poder realizar las instalaciones.

```terminal
arch-chroot /mnt
```

Ahora somos el usuario root de nuestro nuevo sistema, pero funcionando bajo el soporte del instalador. Aun así, todos los cambios que realicemos ahora se implementarán en el nuevo sistema. No sé si es la mejor comparativa pero digamos que es como realizar reparaciones a un barco en dique seco.

### Añadir contraseña

Lo primero que vamos a hacer es darle una contraseña al usuario root.

```terminal
passwd
```

### Cambiar idioma del sistema

Ahora pondremos el idioma en español. Para eso entramos en el fichero <strong>/etc/locale.gen</strong> y vamos bajando hasta encontrar la versión UTF-8 de tu idioma regional. Si eres de España probablemente te interese es_ES.UTF-8.

Cuando encuentres tu idioma elimina la almohadilla de esa línea. Puedes hacerlo con todos los idiomas que quieras.

Ahora aplicaremos los cambios.

```terminal
locale-gen
```

Lo siguiente es configurar la hora local. Para ello haremos un enlace simbólico a la hora de nuestra región. Al ser de España, seleccionaré la hora de Madrid, pero puedes seleccionar cualquier otra región del mundo cambiando de ciudad y continente.

### Establecer horario

```terminal
ln -sf /usr/share/zoneinfo/Europe/Madrid /etc/localtime
```

Actualizamos el reloj físico del ordenador a la nueva hora del sistema.

```terminal
hwclock --systohc --utc
```
### Nombre del equipo

Es muy importante darle un nombre al equipo, esto nos ayudará a identificarlo en la red. Para cambiar el nombre del equipo hay que modificar dos ficheros. El nombre puede ser el que sea (sin espacios ni caracteres especiales), yo llamaré a mi equipo "arch", deberás realizar los mismos pasos sustituyendo "arch" por el nombre del equipo que tu quieras.

```terminal
echo arch > /etc/hostname
```

El siguiente archivo lo tendremos que modificar a mano, tendrá que quedar más o menos así

![](/assets/2021/07/0eJHSk3.png)"
### Modificar el arranque

Vale, por el título puede dar miedo, pero solo tendremos que modificar un archivo, el que le indica al GRUB cómo debe comportarse.

```terminal
nano /etc/default/grub
```

Dentro de este archivo tendremos que cambiar el GRUB_CMDLINE_LINUX indicando que debe desencriptar la partición donde se encuentra el directorio raiz, seguido del nombre del mapper quedando de esta forma: <code>GRUB_CMDLINE_LINUX="cryptdevice=/dev/sda3:root"</code>.

Ahora modificaremos también el script de configuración para la creación del <a href="https://es.wikipedia.org/wiki/Initrd" target="_blank">initrd</a>, básicamente para la creación del núcleo de arranque.

```terminal
nano /etc/mkinitcpio.conf
```

Dentro de este fichero buscaremos el primer "HOOKS" que no esté comentado con una almohadilla y lo modificaremos haciendo que quede de la siguiente forma: <code>HOOKS=(base udev autodetect keyboard keymap consolefont modconf block encrypt filesystem fsck)</code>.

Una vez modificado, ejecutaremos el script para crear el núcleo de arranque Linux.

```terminal
mkinitcpio -P
```

### Desencriptar home al arranque

Como hemos podido observar en el punto anterior, solo hemos indicado que se debe desencriptar el directorio raíz. Para marcar que se debe desencriptar el disco sdb1 debemos modificar el archivo crypttab

```terminal
nano /etc/crypttab
```

Quitaremos la almohadilla de la línea home, cambiaremos la sección "device" por nuestro disco (/dev/sda3) y eliminaremos la línea de password.

Esto hará que nos solicite dos contraseñas al arranque, una para la raíz y otra para el home. Si no queremos que nos solicite la contraseña para el home tendremos que crear un archivo con la contraseña, <a href="https://www.odiseageek.es/gestion-de-permisos-en-linux-cuales-son-y-como-interpretarlos/">cambiar los permisos</a> para que solo pueda leerlo el root y añadir la ubicación en la sección "password".

### Instalar gestor de red

Por último pero no por ello menos importante debemos instalar algún sistema gestor de redes, esto sirve para poder detectar las redes WiFi o configurar las conexiones Ethernet. Es importante instalar únicamente un gestor de redes, porque podría causar conflicto. En este caso instalaremos NetworkManager, uno de los más extendidos.

```terminal
pacman -S networkmanager
```

Ahora debemos indicarle al sistema que lo inicie cada vez que se arranca el sistema. Es importane escribir las mayúsculas donde toca para que funcione.

```terminal
systemctl enable NetworkManager
```

Si mal no recuerdo, al conectar un cable de red la conexión es automática, pero para conectarse a una red WiFi desde la línea de comandos seguimos estos pasos: primero escaneamos en busca de todas las redes disponibles.

```terminal
nmcli device wifi list
```

Después nos conectamos a la red que queramos de la siguiente forma.

```terminal
nmcli device wifi connect "red_wifi" password "contraseña"
```

Sustituimos "red_wifi" por el nombre de la red y "contraseña" por la contraseña de la red, fácil y para toda la familia.

## Configurar GRUB

Ahora toca el último paso y uno de los más delicados. Dependiendo de cómo se haya instalado el sistema, la ubicación de las particiones y si el ordenador es UEFI o BIOS se tendrá que ejecutar de una forma u otra. Tal y como lo hemos hecho aquí, tenemos que hacer lo siguiente.

```terminal
grub-install --boot-directory=/boot --efi-directory=/efi /dev/sda2
```

De esta forma, indicamos dónde está el directorio boot y el directorio efi para que el grub cree las carpetas que necesite dentro de estos. Si todo ha ido bien, dentro de /efi el sistema habrá creado otra carpeta EFI y dentro el nombre de nuestro sistema. Ahora toca plasmar la configuración del grub a ambos directorios de arranque.

```terminal
grub-mkconfig -o /boot/grub/grub.cfg
grub-mkconfig -o /efi/EFI/arch/grub.cfg
```

Una vez esto termine, podremos apagar el ordenador, quitar el USB de instalación y volver a encenderlo. Si todo ha ido correctamente tendrá que salir el GRUB con Arch Linux como opción de arranque y, una vez haya iniciado, solicitará la contraseña de las particiones encriptadas antes de arrancar definitivamente.