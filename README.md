# Guía de Instalación de Arch Linux y BlackArch

En esta guía, te proporcionaremos instrucciones paso a paso sobre cómo instalar Arch Linux desde cero en VMware, utilizando el archivo .iso. Luego, configuraremos una interfaz gráfica Gnome y te mostraremos cómo instalar las dependencias y herramientas de BlackArch.

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
