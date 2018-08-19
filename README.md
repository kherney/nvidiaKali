# nvidiaKali

https://wiki.archlinux.org/index.php/NVIDIA_Optimus
https://wiki.debian.org/Xorg

https://unix.stackexchange.com/questions/315792/installing-proprietary-nvidia-drivers-kali-2016-2
https://medium.com/@jamesmacwhite/installing-the-nvidia-drivers-in-kali-linux-cd3560258e24

# Nvidia initial conf

Copy optimus.desktop file in :
  /usr/share/gdm/greeter/autostart/optimus.desktop
  /etc/xdg/autostart/optimus.desktop

in tty state execute:
  systemctl gdm3 restart
    or
  systemctl gdm3 start
