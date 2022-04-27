# Instalación de Arch Linux ARM en una Mac M1 en UTM de forma manual usando BTRFS

La siguiente es una guía de instalación de Arch Linux en UTM en una Mac M1. Las pruebas de esta guía fueron realizadas en una MacBook Air M1 2020, el 27 de Abril de 2022.

## Descarga de archivos necesarios

Lo primero que debemos hacer es descargar Ubuntu ARM. Nos metemos al sitio web de [Ubuntu](https://cdimage.ubuntu.com/daily-live/current/), en el sitio web veremos varias opciones de descarga. Nosotros vamos a descargar la iso de la imagen:

<img width="700" alt="2" src="https://github.com/AlexisMtzGasca/Arch-UTM-M1/blob/main/images/2.png?raw=true">


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

![UTM-GUI-ISO](https://github.com/AlexisMtzGasca/Arch-UTM-M1/blob/main/images/12.png?raw=true)

Después configuraremos el tamaño del almacenamiento que deseamos dedicarle a la máquina

![UTM-GUI-ISO](https://github.com/AlexisMtzGasca/Arch-UTM-M1/blob/main/images/13.png?raw=true)

Después el asistente nos llevará a configurar si queremos configurar un directorio compartido entre el sistema virtualizado y nuestro sistema anfitrión. En mi caso no compartiré un directorio, así que lo omitiré. Una vez le hayamos dado clic en **Next**, tendremos la siguiente pantalla que es el resumen de la máquina virtual. En este punto, si gustas puedes cambiarle el nombre a la máquina, en mi caso la nombraré como "Linux"

![UTM-GUI-ISO](https://github.com/AlexisMtzGasca/Arch-UTM-M1/blob/main/images/15.png?raw=true)

## Ejecución de la máquina virtual

Una vez que hayamos terminado de configurar nuestra máquina virtual procederemos a ejecutarla. En esta versión de la Daily Build de Ubuntu, tenemos que poner el nombre de usuario como "Ubuntu" y presionar la tecla enter, una vez dentro de Ubuntu, buscaremos la terminal y la abriremos

![UTM-GUI-ISO](https://github.com/AlexisMtzGasca/Arch-UTM-M1/blob/main/images/17.png?raw=true)

En mi caso me conectaré a la máquina por medio de ssh.


