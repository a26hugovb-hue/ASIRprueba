# Manual sencillo de instalación de Ubuntu en una máquina virtual

Guía paso a paso para instalar **Ubuntu Desktop** dentro de una máquina virtual creada con **Oracle VirtualBox**, sin modificar el sistema operativo del ordenador.

---

## Índice

1. [Introducción](#1-introducción)
2. [Requisitos previos](#2-requisitos-previos)
3. [Procedimiento](#3-procedimiento)
   * [3.1 Instalar VirtualBox](#31-instalar-virtualbox)
   * [3.2 Descargar la imagen ISO de Ubuntu](#32-descargar-la-imagen-iso-de-ubuntu)
   * [3.3 Crear la máquina virtual](#33-crear-la-máquina-virtual)
   * [3.4 Instalar Ubuntu](#34-instalar-ubuntu)
   * [3.5 Instalar las Guest Additions](#35-instalar-las-guest-additions)
4. [Comprobación](#4-comprobación)
5. [Resolución de problemas](#5-resolución-de-problemas)
6. [Referencias](#6-referencias)

---

## 1. Introducción

Una **máquina virtual** es un ordenador simulado por software que funciona dentro de nuestro ordenador real (el *anfitrión*). El programa que la gestiona se llama *hipervisor*; en esta guía usaremos **VirtualBox**, que es gratuito.

Instalar Ubuntu en una máquina virtual permite:

* Aprender Linux sin riesgo de borrar datos del ordenador.
* Probar configuraciones y comandos, y volver atrás si algo falla.
* Usar Windows y Ubuntu al mismo tiempo.

![Ubuntu funcionando dentro de una máquina virtual de VirtualBox 7.1.4](https://upload.wikimedia.org/wikipedia/commons/0/01/VirtualBox_7.1.4.png)

*Imagen: Ubuntu ejecutándose como sistema invitado en Oracle VirtualBox 7.1.4. Fuente: [Wikimedia Commons](https://commons.wikimedia.org/wiki/File:VirtualBox_7.1.4.png), Autor: Paowee, licencia GPL.*

---

## 2. Requisitos previos

Antes de empezar, comprueba que tienes lo siguiente:

| Recurso | Mínimo | Recomendado |
|---|---|---|
| Memoria RAM del ordenador | 8 GB | 16 GB |
| Espacio libre en disco | 30 GB | 50 GB |
| Procesador | 64 bits con virtualización | 4 núcleos o más |
| Conexión a Internet | Necesaria | Banda ancha |

Además:

* La **virtualización** (*Intel VT-x* o *AMD-V*) debe estar activada en la BIOS/UEFI.
* Permisos de administrador en el ordenador para instalar programas.

> **Nota:** la máquina virtual usa parte de la RAM y del procesador del ordenador real. Cierra otros programas pesados mientras la utilices.

---

## 3. Procedimiento

### 3.1 Instalar VirtualBox

1. Entra en la [página de descargas de VirtualBox](https://www.virtualbox.org/wiki/Downloads).
2. Pulsa en **Windows hosts** para descargar el instalador.
3. Ejecuta el archivo descargado y pulsa **Siguiente** en todas las pantallas, dejando las opciones por defecto.
4. Acepta la instalación de los controladores de red cuando Windows lo pida.
5. Al terminar, abre **VirtualBox**.

### 3.2 Descargar la imagen ISO de Ubuntu

1. Entra en la [web oficial de Ubuntu Desktop](https://ubuntu.com/download/desktop).
2. Descarga la versión **LTS** más reciente (*Long Term Support*, con 5 años de actualizaciones).
3. Guarda el archivo `.iso` en una carpeta fácil de encontrar, por ejemplo `Descargas`.

> **Consejo:** la versión LTS es la más estable y la recomendada para aprender.

### 3.3 Crear la máquina virtual

1. En VirtualBox, pulsa **Nueva**.
2. Rellena los datos básicos:
    * **Nombre:** `Ubuntu`
    * **Imagen ISO:** selecciona el archivo descargado en el paso anterior.
    *  Marca la casilla **Omitir instalación desatendida** (*Skip Unattended Installation*) para hacer la instalación nosotros mismos.
3. Asigna el hardware y el disco según esta tabla:

| Parámetro | Valor recomendado | Para qué sirve |
|---|---|---|
| Memoria base | 4096 MB | RAM que usará Ubuntu |
| Procesadores | 2 CPU | Núcleos asignados a la máquina |
| Disco duro virtual | 25 GB (reservado dinámicamente) | Espacio para el sistema y los archivos |

4. Revisa el resumen y pulsa **Terminar**.

> **Importante:** con *reservado dinámicamente* el disco virtual solo ocupa el espacio que realmente se usa, no los 25 GB desde el principio.

### 3.4 Instalar Ubuntu

1. Selecciona la máquina **Ubuntu** y pulsa **Iniciar**.
2. En el menú de arranque elige **Try or Install Ubuntu**.
3. Selecciona el **idioma** (*Español*) y la **distribución del teclado** (*Español*).
4. Elige **Instalar Ubuntu** y la **instalación interactiva** con la selección de aplicaciones *por defecto*.
5. En el tipo de instalación, marca **Borrar disco e instalar Ubuntu**.
6. Elige la **zona horaria** (*Madrid*).
7. Crea el usuario: nombre, nombre del equipo y contraseña.
8. Pulsa **Instalar** y espera a que termine (unos 10–20 minutos).
9. Pulsa **Reiniciar ahora**. Si aparece un mensaje pidiendo quitar el medio de instalación, pulsa `Enter`.

> **Aviso:** *Borrar disco* solo afecta al **disco virtual** de la máquina, no al disco del ordenador real. Tus archivos de Windows están a salvo.

### 3.5 Instalar las Guest Additions

Las *Guest Additions* mejoran la integración: ajuste automático de la pantalla, portapapeles compartido y mejor rendimiento gráfico.

1. Inicia sesión en Ubuntu y abre una **Terminal** con `Ctrl + Alt + T`.
2. Actualiza el sistema e instala las herramientas necesarias:

```bash
sudo apt update
sudo apt upgrade -y
sudo apt install -y build-essential dkms linux-headers-$(uname -r)
```

3. En el menú de la ventana de VirtualBox, pulsa **Dispositivos → Insertar imagen de CD de las Guest Additions**.
4. Ejecuta el instalador desde la terminal:

```bash
cd /media/$USER/VBox_GAs_*
sudo ./VBoxLinuxAdditions.run
```

5. Reinicia la máquina virtual:

```bash
sudo reboot
```

6. Para activar el portapapeles compartido: **Dispositivos → Portapapeles compartido → Bidireccional**.

---

## 4. Comprobación

Para confirmar que todo funciona correctamente, abre una terminal y ejecuta estos comandos:

```bash
lsb_release -a          # Muestra la versión de Ubuntu instalada
ip addr                 # Muestra las interfaces de red y la dirección IP
ping -c 4 ubuntu.com    # Comprueba la conexión a Internet
df -h                   # Muestra el espacio usado y libre del disco
```

La instalación es correcta si:

* `lsb_release -a` muestra la versión de Ubuntu descargada.
* `ip addr` muestra una interfaz (por ejemplo `enp0s3`) con una dirección IP del tipo `10.0.2.15`.
* `ping` recibe respuesta sin pérdida de paquetes.
* Al cambiar el tamaño de la ventana, el escritorio de Ubuntu se adapta automáticamente (*Guest Additions* funcionando).

---

## 5. Resolución de problemas

| Problema | Causa probable | Solución |
|---|---|---|
| Error *VT-x/AMD-V is not available* | Virtualización desactivada en la BIOS | Activar *Intel VT-x* o *AMD-V* en la BIOS/UEFI |
| Solo aparecen versiones de 32 bits | Virtualización desactivada o Hyper-V activo | Activar la virtualización y desactivar Hyper-V en Windows |
| Pantalla pequeña que no se ajusta | Faltan las *Guest Additions* | Repetir el [paso 3.5](#35-instalar-las-guest-additions) |
| Ubuntu va muy lento | Poca RAM o CPU asignada | Con la máquina apagada: **Configuración → Sistema** y aumentar los valores |
| Vuelve a arrancar el instalador | La ISO sigue conectada | **Configuración → Almacenamiento** y quitar la ISO de la unidad óptica |
| No hay Internet | Adaptador de red mal configurado | **Configuración → Red** y elegir **NAT** |

> **Consejo:** antes de hacer cambios importantes, crea una **instantánea** (*snapshot*) desde **Máquina → Tomar instantánea**. Así podrás volver a ese estado si algo sale mal.

---

## 6. Referencias

* [Descargas de VirtualBox](https://www.virtualbox.org/wiki/Downloads)
* [Manual de usuario de VirtualBox](https://www.virtualbox.org/manual/)
* [Descarga de Ubuntu Desktop](https://ubuntu.com/download/desktop)
* [Tutorial oficial: instalar Ubuntu Desktop](https://ubuntu.com/tutorials/install-ubuntu-desktop)
* [Documentación oficial de Ubuntu](https://help.ubuntu.com/)
* [Imagen de la introducción: *VirtualBox 7.1.4.png*, Wikimedia Commons](https://commons.wikimedia.org/wiki/File:VirtualBox_7.1.4.png) 
---