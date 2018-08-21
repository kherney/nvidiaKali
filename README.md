# Instalación Nvidia en Laptops

Instalar CUDA software en Kali-Linux laptops con tarjetas gráficas con la tecnología NVIDIA Optimus. La presente guía es una recopilación de algunos post dedicados a la instalación de Nvidia en kali-LInux. Las URL's de dichos posters estan en Acknowledgments.

## Getting Started

El procedimeinto de intalación esta comprobado en *Kali linux Kali Linux 64 Bit version 2018-2* con *kernel 4.17* y tarjeta grafica *NVIDIA GEFORCE 940MX* . Importante ! Aclarar que el gestor de pantalla por defecto en Kali linux es **gdm3** como lo es **lightdm** en ubuntu. La configuración de X11-Server difiere del tipo del gestor de panatalla "Display Manager" utilizado.

### Prerequisites

Antes de instalar drivers de Nvidia, se debe actualizar las listas (It takes time !) como actualizar kernel.

```
lspci | egrep "VGA|3D"
apt update
apt dist-upgrade
reboot
apt install linux-headers-$(uname -r)
reboot
```
ALgunos casos el penultimo comando no funciona. Para ello debe verificar que la version del kernel **uname -r** sea igual a las versiones de linux-headers disponibles en apt.

```
uname -r
apt search linux-headers
```

### Installing

Instalar NVIDIA drivers.

```
apt install nvidia-kernel-dkms nvidia-xconfig nvidia-settings
apt install nvidia-vdpau-driver vdpau-va-driver mesa-utils
reboot
```

## Configuration

Al instalar Nvidia, debe indicarle ni mas ni menos la ubicaciín de la tarjeta Nvidia en la configuracion de XORG y adaptar dicha configuración al tipo de gestor de pantalla.

Al reiniciar su laptop e iniciar Kali-Linux, la pantalla quedará en black Screen, de modo que debe iniciar en modo tty. Para inducir el modo tty solo basta tipear >[Ctrl]+[Alt]+[F2] y logearse en kali Linux.

La configuración depende de tu gestor de pantalla. Esta guía esta basada en GDM3. Para verificar su Display Manager, type:

```
cat /etc/X11/default-display-manager
```

### First Step: XORG conf

Debe conocer BUS ID de su placa NVIDIA. En mi caso es PCI:01:00:00.

```
nvidia-xconfig --query-gpu-info

```
xorg.conf puede ser generado desde el Xserver o utilizar el archivo xorg del repositorio. Para generar el archivo de configuración, debe asegurarse que su gestor de pantalla esté inactivo.

**Generar Xorg.conf**

```
service gdm3 status
service gdm3 stop
cd /etc/X11/
Xorg -configure
```
**Usar Xorg.conf**

```
service gdm3 status
service gdm3 stop
cp ~/nvidiaKali/xorg.conf /etc/X11/xorg.conf
nano /etc/X11/xorg.conf
```
Dentro del archivo xorg.conf debe en la seccion *Device* debe cambiar su **BusID** de la placa grafica.

```
Section "Device"
    Identifier     "nvidia"
    Driver         "nvidia"
    VendorName     "NVIDIA Corporation"
    BusID	         "1:0:0"
    Option         "AllowEmptyInitialConfiguration"
    Option         "ModeValidation" "AllowNonEdidModes"
EndSection

```

### Second Step

Adaptar la configuración de nuestro gestor de pantalla a la nueva tarjeta grafica.

```
cp ~/nvidiaKali/optimus.desktop /usr/share/gdm/greeter/autostart/optimus.desktop
cp ~/nvidiaKali/optimus.desktop /etc/xdg/autostart/optimus.desktop
service gdm3 start
```
## Something Else !

```
apt install nvidia-smi nvidia-opencl-common
nvidia-msi
xrandr --listproviders
```
## Authors and Acknowledgments

* **Kevin Herney** - *Initial work -2018-2* - [nvidiaKali](https://github.com/kherney/nvidiaKali)

See also the list of posters:

* [StackExchange](https://unix.stackexchange.com/questions/315792/installing-proprietary-nvidia-drivers-kali-2016-2) - by Daniel Lane
* [Medium](https://medium.com/@jamesmacwhite/installing-the-nvidia-drivers-in-kali-linux-cd3560258e24) - by James White Medium Website
* [XORG](https://wiki.debian.org/Xorg/) - XORG Website
* [NVIDIA Optimus](https://wiki.archlinux.org/index.php/NVIDIA_Optimus) - archLinux Website
