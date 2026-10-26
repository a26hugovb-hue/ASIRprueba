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
6. [Modos de red en VirtualBox](#6-modos-de-red-en-virtualbox)
7. [Referencias](#7-referencias)

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

Una vez instalados Ubuntu y las *Guest Additions*, conviene comprobar que el sistema, los recursos asignados y la red funcionan correctamente. Abre una terminal con `Ctrl + Alt + T` y ejecuta los siguientes comandos.

### 4.1 Sistema operativo

```bash
lsb_release -a   # Muestra la versión de Ubuntu instalada
uname -r         # Muestra la versión del núcleo (kernel) de Linux
```

### 4.2 Recursos de la máquina virtual

```bash
free -h          # Muestra la memoria RAM total, usada y libre
nproc            # Muestra el número de procesadores disponibles
df -h            # Muestra el espacio usado y libre del disco
```

Estos valores deben coincidir con el hardware asignado en el [paso 3.3](#33-crear-la-máquina-virtual).

### 4.3 Red

```bash
ip addr               # Muestra las interfaces de red y la dirección IP
ping -c 4 ubuntu.com  # Comprueba la conexión a Internet
```

### 4.4 Guest Additions

```bash
VBoxControl --version   # Muestra la versión de las Guest Additions instaladas
```

### 4.5 Resultados esperados

| Comando | Resultado correcto |
|---|---|
| `lsb_release -a` | Aparece *Ubuntu* y la versión LTS descargada |
| `uname -r` | Un número de versión del kernel (depende de la versión de Ubuntu) |
| `free -h` | Un total cercano a 4 GB (algo menos, porque el sistema reserva una parte) |
| `nproc` | `2` |
| `df -h` | La partición `/` con un tamaño cercano a 25 GB |
| `ip addr` | Una interfaz (por ejemplo `enp0s3`) con una IP del tipo `10.0.2.15` en modo NAT |
| `ping -c 4 ubuntu.com` | 4 paquetes enviados, 4 recibidos y `0% packet loss` |
| `VBoxControl --version` | Un número de versión, por ejemplo `7.1.4r...` |

Además, al cambiar el tamaño de la ventana, el escritorio de Ubuntu debe adaptarse automáticamente.

> **Nota:** si algún resultado no coincide, consulta la sección [5. Resolución de problemas](#5-resolución-de-problemas).

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

## 6. Modos de red en VirtualBox

La conexión de red de la máquina virtual se configura con la máquina apagada en **Configuración → Red → Adaptador 1 → Conectado a**.

| Modo | ¿Tiene Internet? | ¿Se comunica con el anfitrión? | Uso típico |
|---|---|---|---|
| **NAT** | Sí | No directamente | Opción por defecto. Navegar e instalar paquetes |
| **Red NAT** | Sí | No directamente | Varias máquinas virtuales que se ven entre sí y salen a Internet |
| **Adaptador puente** | Sí | Sí | La máquina aparece en la red de casa como un equipo más |
| **Red interna** | No | No | Solo se comunican las máquinas virtuales entre sí |
| **Solo anfitrión** | No | Sí | Red privada entre el anfitrión y las máquinas virtuales |

### Comprobar la configuración de red

Desde una terminal de Ubuntu:

```bash
ip addr             # Muestra la dirección IP de cada interfaz
ip route            # Muestra la puerta de enlace (router) que usa la máquina
ping -c 4 8.8.8.8   # Comprueba si hay salida a Internet
```

* En modo **NAT**, la IP suele ser `10.0.2.15`.
* En modo **adaptador puente**, la IP será de la misma red que el ordenador real (por ejemplo `192.168.1.X`).

> **Consejo:** para las prácticas normales usa **NAT**. Cambia a **adaptador puente** solo si necesitas que otros equipos de la red vean la máquina virtual.

---

## 7. Referencias

* [Descargas de VirtualBox](https://www.virtualbox.org/wiki/Downloads)
* [Manual de usuario de VirtualBox](https://www.virtualbox.org/manual/)
* [Descarga de Ubuntu Desktop](https://ubuntu.com/download/desktop)
* [Tutorial oficial: instalar Ubuntu Desktop](https://ubuntu.com/tutorials/install-ubuntu-desktop)
* [Documentación oficial de Ubuntu](https://help.ubuntu.com/)
* [Imagen de la introducción: *VirtualBox 7.1.4.png*, Wikimedia Commons](https://commons.wikimedia.org/wiki/File:VirtualBox_7.1.4.png) 
---