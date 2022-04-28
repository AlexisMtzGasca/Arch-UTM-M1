# Instalación de Arch Linux ARM en una Mac M1 en UTM de forma manual usando BTRFS

La siguiente es una guía de instalación de Arch Linux en UTM en una Mac M1. Las pruebas de esta guía fueron realizadas en una MacBook Air M1 2020, el 27 de Abril de 2022.

## Descarga de archivos necesarios

Lo primero que debemos hacer es descargar Ubuntu ARM. Nos metemos al sitio web de [Ubuntu](https://cdimage.ubuntu.com/daily-live/current/), en el sitio web veremos varias opciones de descarga. Nosotros vamos a descargar la iso de la imagen:
![Ubuntu-Website](https://github.com/AlexisMtzGasca/Arch-UTM-M1/blob/main/images/2.png?raw=true)
Una vez descargada la imagen ISO de Ubuntu ARM, procederemos a descargar UTM en caso de no tenerlo instalado. Nos metemos al sitio web de [UTM](https://mac.getutm.app)
![UTM-Website](https://github.com/AlexisMtzGasca/Arch-UTM-M1/blob/main/images/4.png?raw=true)
Cuando la imagen DMG de UTM haya sido descargada, la abrimos y copiamos la aplicación a la carpeta "Aplicaciones"
![UTM-to-Apps](https://github.com/AlexisMtzGasca/Arch-UTM-M1/blob/main/images/6.png?raw=true)
## Preparación de la máquina virtual
Iniciamos la aplicación de UTM y tendremos una interfaz como la siguiente
![UTM-GUI](https://github.com/AlexisMtzGasca/Arch-UTM-M1/blob/main/images/7.png?raw=true)
Le damos clic en **Create a New Virtual Machine** y tendremos la siguiente pantalla
![UTM-GUI-Create](https://github.com/AlexisMtzGasca/Arch-UTM-M1/blob/main/images/8.png?raw=true)
Damos clic en **Virtualize** y veremos la siguiente interfaz
![UTM-GUI-Virtualize](https://github.com/AlexisMtzGasca/Arch-UTM-M1/blob/main/images/9.png?raw=true)
Vamos a darle clic en **Linux** y tendremos la siguiente interfaz
![UTM-GUI-Linux](https://github.com/AlexisMtzGasca/Arch-UTM-M1/blob/main/images/10.png?raw=true)
Le damos clic en **Browse** y navegamos hasta la ubicación de la [Imagen ISO de Ubuntu](https://cdimage.ubuntu.com/daily-live/current/) que descargamos previamente, la seleccionamos y damos clic en **Open**
![UTM-GUI-ISO](https://github.com/AlexisMtzGasca/Arch-UTM-M1/blob/main/images/11.png?raw=true)
Una vez cargada la imagen ISO, damos clic en **Next** y nos saldrá una pantalla donde configuraremos el tamaño de la memoria que deseamos asignarle a la máquina virtual, en mi caso dedicaré 4000MB
![UTM-RAM](https://github.com/AlexisMtzGasca/Arch-UTM-M1/blob/main/images/12.png?raw=true)
Después configuraremos el tamaño del almacenamiento que deseamos dedicarle a la máquina, en mi caso le daré un tamaño de 30GB
![UTM-DD](https://github.com/AlexisMtzGasca/Arch-UTM-M1/blob/main/images/13.png?raw=true)
Después el asistente nos llevará a configurar si queremos configurar un directorio compartido entre el sistema virtualizado y nuestro sistema anfitrión. En mi caso no compartiré un directorio, así que lo omitiré. Una vez le hayamos dado clic en **Next**, tendremos la siguiente pantalla que es el resumen de la máquina virtual. En este punto, si gustas puedes cambiarle el nombre a la máquina, en mi caso la nombraré como "Linux"
![UTM-Resume](https://github.com/AlexisMtzGasca/Arch-UTM-M1/blob/main/images/15.png?raw=true)
## Ejecución de la máquina virtual
Una vez que hayamos terminado de configurar nuestra máquina virtual procederemos a ejecutarla. En esta versión de la Daily Build de Ubuntu, tenemos que poner el nombre de usuario como "ubuntu" y presionar la tecla enter, una vez dentro de Ubuntu, buscaremos la terminal y la abriremos
![Ubuntu-GUI](https://github.com/AlexisMtzGasca/Arch-UTM-M1/blob/main/images/18.png?raw=true)
En mi caso me conectaré a la máquina por medio de ssh.
![ssh-active](https://github.com/AlexisMtzGasca/Arch-UTM-M1/blob/main/images/19.png?raw=true)
## Instalación de Arch Linux
Para empezar con la instalación de Arch, primero vamos a ingresar como root con  el siguiente comando
```
sudo -s 
```
Una vez como sudo, abriremos el archivo de fuentes de APT (sources.list) ubicado en /etc/apt
```
nano /etc/apt/sources.list
```
Y le añadiremos "universe multiverse" en la línea ```deb http://ports.ubuntu.com/ubuntu-ports/jammy main restricted```tal como de muestra en la siguiente imagen
![Ubuntu-repo](https://github.com/AlexisMtzGasca/Arch-UTM-M1/blob/main/images/20.png?raw=true)
Posteriormente, actualizaremos los repositorios
```
sudo apt update
```
Vamos a instalar los scripts de instalación de Arch, las utilidades necesarias para BTRFS y WGET
```
sudo apt install arch-install-scripts btrfs-progs wget
```
### Creación de particiones
Posteriormente crearemos las particiones necesarias para el sistema. Se trata de 3 particiones: una efi, una partición de intercambio (swap) y otra para la partición raíz, esto lo haremos con CFDISK
```
cfdisk
```
Cuando abramos Cfdisk, veremos la siguiente interfaz que nos pedirá qué formato le daremos a la tabla de particiones, nosotros usaremos GPT
![CFDISK-1](https://github.com/AlexisMtzGasca/Arch-UTM-M1/blob/main/images/21.png?raw=true)
Al darle clic en enter, nos aparecerá la siguiente pantalla que nos mostrará el espacio disponible para crear las particiones, navegaremos en las opciones a través de las flechas. Damos clic en New y le asignaremos un tamaño de 500MB, escribiendo **500M**
![CFDISK-1st-part](https://github.com/AlexisMtzGasca/Arch-UTM-M1/blob/main/images/22.png?raw=true)
Posteriormente seleccionaremos el espacio libre y asignaremos una nueva partición de 2GB, escribiendo **2G**
![CFDISK-2nd-part](https://github.com/AlexisMtzGasca/Arch-UTM-M1/blob/main/images/23.png?raw=true)
Crearemos una nueva partición en el espacio disponible, sólo le daremos clic sin asignar espacio para que la partición tenga el tamaño total del espacio libre
![CFDISK-GUI-2](https://github.com/AlexisMtzGasca/Arch-UTM-M1/blob/main/images/27.png?raw=true)
Posteriormente cambiaremos los tipos de partición para nuestro sistema. Para la primera partición le daremos un tipo de partición EFI
![EFI](https://github.com/AlexisMtzGasca/Arch-UTM-M1/blob/main/images/25.png?raw=true)
Para la segunda partición, le daremos un tipo **Linux Swap**
![Swap](https://github.com/AlexisMtzGasca/Arch-UTM-M1/blob/main/images/26.png?raw=true)
Y dejaremos la tercera partición con el tipo predeterminado que es **Linux Filesystem**. Finalmente escribiremos en **Write** y escribimos **yes** para confirmar los cambios en el disco

Para confirmar que nuestras particiones son correctas, podemos usar el comando ```lsblk``` para confirmarlo
![lsblk](https://github.com/AlexisMtzGasca/Arch-UTM-M1/blob/main/images/38.png?raw=true)

Posteriormente le daremos formato a las 3 particiones que creamos
```
mkfs.fat -F32 /dev/vda1 #Para darle formato de Fat32 a la partición EFI
mkswap /dev/vda2 #Para darle formato Swap a la segunda partición
mkfs.btrfs /dev/vda3 #Para darle formato BTRFS a la partición raíz
swapon /dev/vda2 #Para que el sistema active la swap de la partición 2
```

### Creación de subvolúmenes BTRFS

Vamos a crear 6 subvolúmenes para las carpetas del sistema y 1 más por si en el futuro deseamos usar snapshots con Snapper


Para crear subvolúmenes, primero vamos a montar la partición raíz en /mnt
```
mount /dev/vda3 /mnt
```

Una vez montada la partición, procederemos a crear los subvolúmenes
```
btrfs su cr /mnt/@
btrfs su cr /mnt/@home
btrfs su cr /mnt/@srv
btrfs su cr /mnt/@tmp
btrfs su cr /mnt/@var
btrfs su cr /mnt/@opt
btrfs su cr /mnt/@.snapshots
```
Una vez creados los subvolúmenes, vamos a desmontar la partición
```
umount /mnt
```
Montaremos todos los subvolúmenes con las siguiente opciones:
- noatime: No actualiza los tiempos de acceso de inodos en este sistema de archivos. Esto acelera las lecturas ya que los metadatos del tiempo de acceso no se actualizan.
- compress: El tipo de compresión que usaremos para el subvolúmen, en este caso usaré zstd
- space_cache: Opciones para controlar la caché de espacio libre. La caché de espacio libre mejora en gran medida el rendimiento al leer el espacio libre del grupo de bloques en la memoria. Sin embargo, la gestión de la caché de espacio consume algunos recursos, incluida una pequeña cantidad de espacio en disco.
- subvol: Nombre del subvolúmen creado

Primero empezaremos con el subvolúmen "@" que corresponde al subvolúmen de nuestra partición raíz
```
mount -o noatime,compress=zstd,space_cache=v2,subvol=@ /dev/vda3 /mnt
```
Posteriormente crearemos los directorios dentro de /mnt para montar los otros subvolúmenes y los montaremos
```
mkdir -v /mnt/{home,srv,tmp,var,opt,.snapshots}
mount -o noatime,compress=zstd,space_cache=v2,subvol=@home /dev/vda3 /mnt/home
mount -o noatime,compress=zstd,space_cache=v2,subvol=@srv /dev/vda3 /mnt/srv
mount -o noatime,compress=zstd,space_cache=v2,subvol=@tmp /dev/vda3 /mnt/tmp
mount -o noatime,compress=zstd,space_cache=v2,subvol=@var /dev/vda3 /mnt/var
mount -o noatime,compress=zstd,space_cache=v2,subvol=@opt /dev/vda3 /mnt/opt
mount -o noatime,compress=zstd,space_cache=v2,subvol=@.snapshots /dev/vda3 /mnt/.snapshots
```
### Descarga de Arch Linux

Para empezar con la descarga de Arch Linux, vamos a usar wget, primero nos iremos a /mnt y procederemos con la descarga
```
cd /mnt
wget http://os.archlinuxarm.org/os/ArchLinuxARM-aarch64-latest.tar.gz
```
![WGET-Arch](https://github.com/AlexisMtzGasca/Arch-UTM-M1/blob/main/images/29.png?raw=true)
Posteriormente extraeremos el tar.gz de Arch con el siguiente comando y lo borramos
```
tar xvf ArchLinuxARM-aarch64-latest.tar.gz && rm -rf ArchLinuxARM-aarch64-latest.tar.gz
```
Una vez extraído completamente el sistema, procederemos con la configuración del sistema

### Configuración de Arch Linux

Primeramente vamos a generar nuestra tabla de particiones (fstab) para que el init reconozca que particiones se tienen que montar, para esto, usaremos el programa genfstab

```
genfstab -U /mnt > /mnt/etc/fstab 
```
Posteriormente vamos a crear el enlace simbólico para que nuestra zona horaria este correcta. En mi caso es la Ciudad de México, pero tú puedes revisar la ubicación de tu zona horaria
```
ln -sf /mnt/usr/share/zoneinfo/America/Mexico_City /mnt/etc/localtime
```
Después seleccionaremos el idioma del sistema, editando con nano descomentando el idioma y la región que te corresponde, en mi caso es Español de México, tal como aparece en la imágen. Guardamos con Control + O y salimos con Control + X
```
nano /mnt/etc/locale.gen  
```
![locale](https://github.com/AlexisMtzGasca/Arch-UTM-M1/blob/main/images/31.png?raw=true)
A continuación vamos a crear el archivo locale.conf
```
echo "LANG=es_MX.UTF-8" > /mnt/etc/locale.conf
```
Después vamos a crear el archivo vconsole.conf para elegir nuestra distribución de teclado
```
echo "KEYMAP=la-latin1" > /mnt/etc/vconsole.conf
```
Ahora le asignaremos un nombre al equipo, en mi caso se llamará Arch
```
echo “Arch" > /mnt/etc/hostname
```
Posteriormente editaremos el archivo /mnt/etc/hosts de la siguiente forma. Nótese que se tiene que sustituir la palabra Arch por el nombre que hayas elegido para tu equipo
```
nano /mnt/etc/hosts
```
![hosts](https://github.com/AlexisMtzGasca/Arch-UTM-M1/blob/main/images/30.png?raw=true)

Ahora vamos a generar el archivo /etc/adjtime y cambiar la contraseña del usuario root
```
arch-chroot /mnt /bin/bash -c "hwclock --systyohc && passwd root"
```
A continuación vamos a crear un usuario normal para el sistema, en mi caso será alexis
```
arch-chroot /mnt /bin/bash -c "useradd -mG wheel alexis && passwd alexis"
```
Posteriormente vamos a inicializar Pacman para instalación de paquetes, confirmar el repositorio de archlinuxarm, e instalar efibootmgr, las utilidades de btrfs, paquetes de desarrollo y utilidades para los sistemas de archivos DOS (FAT32), además de generar las locales de nuestro sistema, y veremos cómo los paquetes se instalan
```
arch-chroot /mnt /bin/bash -c "pacman-key --init && pacman-key --populate archlinuxarm && pacman -Syu --noconfirm efibootmgr btrfs-progs base-devel dosfstools && locale-gen”
```
![pacman](https://github.com/AlexisMtzGasca/Arch-UTM-M1/blob/main/images/32.png?raw=true)
Casi por terminar, editaremos el archivo sudoers para que nuestro usuario tenga permisos de usar sudo. Descomentaremos **wheel ALL=(ALL:ALL) ALL** que es el segundo renglón, quitándole el "#" que aparece en dicho renglón, tal como se muestra en la siguiente imagen.
```
arch-chroot /mnt /bin/bash -c “EDITOR=nano visudo” 
```
Finalmente crearemos la entrada EFI para el sistema con efibootmgr
```
arch-chroot /mnt /bin/bash -c "efibootmgr --disk /dev/vda --part 1 --create --label 'Arch Linux' --loader /Image --unicode 'root=UUID=$(blkid -s UUID -o value /dev/vda3) rw rootflags=subvol=@ initrd=\initramfs-linux.img' --verbose"
```
*Nótese que la primera parte dice que se va a crear la entrada efi en la partición 1, y que las rootflags indican que el volumen a usar es "@" indiciando que es el subvolúmen para el sistema raíz*

Posteriormente salimos de chroot y apagamos el sistema, después quitamos el archivo ISO de Ubuntu de la máquina virtual, dándole clic en "Clear" como en la siguiente imagen
![UTM-final](https://github.com/AlexisMtzGasca/Arch-UTM-M1/blob/main/images/35.png?raw=true)

Ahora iniciamos la máquima virtual y podremos ver la pantalla de inicio de Arch
![Arch-final](https://github.com/AlexisMtzGasca/Arch-UTM-M1/blob/main/images/36.png?raw=true)

## Instalación y configuración de Snapper
```TODO
```
## Instalación de entornos gráficos
```TODO
```
