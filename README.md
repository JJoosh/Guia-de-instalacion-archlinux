# Guía de Instalación de Arch Linux y BlackArch

En esta guía, te proporcionaremos instrucciones paso a paso sobre cómo instalar Arch Linux desde cero en VMware, utilizando el archivo .iso. Luego, configuraremos una interfaz gráfica Gnome y te mostraremos cómo instalar las dependencias y herramientas de BlackArch.

#### Nota: Alternativamente, puedes realizar la instalación de Arch Linux utilizando el instalador gráfico oficial llamado "archinstall", que simplifica en gran medida el proceso de instalación, especialmente para usuarios principiantes. La guía que será proporcionad es una forma más manual de instalar Arch Linux y puede brindarte un mayor entendimiento de los aspectos internos del sistema. Sin embargo, el instalador "archinstall" es una excelente opción si prefieres una instalación más guiada y amigable para principiantes. Puedes elegir el método que mejor se adapte a tus necesidades y nivel de experiencia.


## Instalación de Arch Linux

Comenzaremos con la instalación de Arch Linux.

### 1. Preparación inicial

Inicia el proceso de instalación y, una vez que tengas una terminal como root:

```bash
# Para configurar el teclado en español
loadkeys es

# Verificar la conexión a Internet
ping -c 1 google.com
```

## 2. Particionamiento del disco
Utilizaremos cfdisk para particionar el disco. En una máquina virtual, sigue estos pasos:

* Crea tres particiones nuevas: boot, root y swap.
* La primera partición debe tener 512 MB.
* La segunda partición debería ocupar la mayor parte del espacio disponible.
* La tercera partición deberá ocupar los 4.5 GB restantes y se configurará como "Linux swap/solaris".
* Después de crear las particiones, asegúrate de guardar los cambios en cfdisk.

## 3. Validación de particiones
Verifica que las particiones se hayan creado correctamente con el comando:

```bash
lsblk
```

## 4. Formateo de particiones
Formatea las particiones con los siguientes comandos:

``` bash
# Formatear la partición boot
mkfs.vfat -F 32 /dev/sda1

# Formatear la partición root
mkfs.ext4 /dev/sda2

# Formatear la partición swap
mkswap /dev/sda3
swapon /dev/sda3
```
## 5. Montaje de particiones
Monta las particiones formateadas:

``` bash
# Montar la partición root en /mnt
mount /dev/sda2 /mnt

# Crear la carpeta boot dentro de /mnt y montar la partición boot
mkdir /mnt/boot
moun
t /dev/sda1 /mnt/boot
``` 

## 6. Instalación de paquetes del sistema
Instala los paquetes básicos del sistema:

```bash
pacstrap -i /mnt base base-devel linux linux-firmware git nano networkmanager grub
```
En caso de errores, puedes usar:

```bash
pacman -Sy && pacman -S archlinux-keyring
```

7. Generación del archivo fstab
Genera el archivo fstab con el siguiente comando:

```bash
genfstab -U /mnt >> /mnt/etc/fstab
```

## 8. Acceso al nuevo sistema
Ingresa al nuevo sistema con arch-chroot:

```bash
arch-chroot /mnt
```

9. Contraseña de root
Asigna una contraseña al usuario root:

```bash 
passwd
``` 

## 10. Creación de un usuario
Crea un usuario con el nombre que desees:

```bash
useradd -m nombre_de_usuario
```

## 11. Configuración de zona horaria
Establece la zona horaria de acuerdo a tu ubicación:

```bash
ln -sf /usr/share/zoneinfo/(Region)/(Ciudad) /etc/localtime
```

## 12. Actualización del reloj del sistema
Actualiza el reloj del sistema:

```bash
hwclock --systohc
```

## 13. Configuración del idioma
Edita el archivo locale.gen para configurar el idioma:

```bash
nano /etc/locale.gen
```

Descomenta las líneas correspondientes a tu idioma (por ejemplo, en_US y es_ES). Luego, ejecuta:

```bash 
locale-gen
``` 

## 14. Configuración del teclado
Si deseas mantener el mismo layout de teclado en cada inicio de sesión, crea o edita el archivo vconsole.conf:

```bash 
echo "KEYMAP=es" > /etc/vconsole.conf
``` 

## 15. Nombre del host
Cambia el nombre de tu host:

```bash
echo "nombre_de_tu_equipo" > /etc/hostname
```

## 16. Configuración del archivo /etc/hosts
Edita el archivo /etc/hosts y agrega las siguientes líneas:

```bash
127.0.0.1 	localhost
::1		localhost
127.0.0.1	(nombre_de_tu_equipo).localhost (nombre_de_tu_equipo)
```
## 17. Generación de imágenes del sistema
Genera las imágenes del sistema con:

```bash 
mkinitcpio -P
```

## 18. Instalación de GRUB
Instala el gestor de arranque GRUB:

```bash
grub-install /dev/sda
grub-mkconfig -o /boot/grub/grub.cfg
```

## 19. Reinicio del sistema
Reinicia el sistema:

```bash 
reboot now
```

## 20. Agregar usuario a grupo "wheel"
Una vez que hayas reiniciado, agrega tu usuario al grupo "wheel" y descomenta la línea correspondiente en el archivo /etc/sudoers:

```bash 
usermod -aG wheel nombre_de_usuario
``` 

## 21. Configuración de la red
Para activar los servicios de red sin reiniciar, ejecuta los siguientes comandos:

```bash
systemctl enable NetworkManager
systemctl start NetworkManager
``` 

## 22. Instalación de entorno gráfico y otras herramientas
Instala el entorno gráfico y otras herramientas según tus preferencias:

```bash
pacman -S xorg xorg-server gnome kitty sudo
```

## 23. Habilitar GDM (Gnome Display Manager)
Para iniciar en la interfaz gráfica de Gnome al arrancar, utiliza el siguiente comando:

```bash 
systemctl enable gdm
```

## 24. Configuración adicional para VMware (opcional)
Si estás utilizando VMware, instala los paquetes necesarios para la interfaz gráfica:

```bash 
pacman -S gtkmm open-vm-tools xf86-video-vmware xf86-input-vmmouse
systemctl enable vmtoolsd
reinicia la máquina con `reboot now`
``` 

### ¡Listo! Has instalado Arch Linux y configurado tu sistema base.
### Es recomendable crear un snapshot de la máquina base antes de personalizarla para poder volver a un estado inicial estable si algo sale

## Instalación de herramientas BlackArch

Para instalar herramientas de BlackArch en tu sistema de Arch Linux, sigue estos pasos:

Copia el script de instalación:

```bash 
curl -O https://blackarch.org/strap.sh
```

Concede permisos de ejecución al script:

```bash 
chmod +x strap.sh
``` 

Ejecuta el script de instalación con sudo:

```bash 
sudo ./strap.sh
``` 

Esto instalará el repositorio de BlackArch en tu sistema. Luego, puedes usar sudo pacman -S para instalar herramientas específicas, por ejemplo:

```bash 
sudo pacman -S metasploit
``` 

Este proceso es válido para cualquier otra herramienta que desees instalar.

¡Tu sistema está listo para usar tanto Arch Linux como las herramientas de BlackArch!

