---
title: Fundamentos Básicos de Linux
published: true
---

**Linux Arquitectura**

El *Sistema Operativo(OS)* de Linux se puede dividir en ciertas **capas**:

| Estructura     | Descripción                                                                                 |
|:---------------|:--------------------------------------------------------------------------------------------|
| Hardware       | Dispositivo periféricos, tales como la memoria RAM del sistema, dico duro, CPU, entre otros. |
| Kernel         | El núcleo del sistema operativo Linux, cuya funciones es virtualizar y controlar los recursos comunes de Hardware, como la memoria asignada, acceso a datos, y otros. El kernel da a cada proceso con sus propios recursos virtuales y evita/mitiga los conflictos entre los diferentes procesos.|
| Terminal(Shell)| Es una interfaz de linea de comandos(CLI, command-line interface), también conocido como **SHELL**, es una terminal que se puede introducir comandos a ejecutar. |
| Utilidades del Sistema | El usuario tiene a disposición todas las funcionalidades del sistema operativo. |


* * *

**Jerarquía del sistema de archivos**
 
La Estructura del *sistema de archivo* o *Raiz* es una Jerarquía de tipo árbol, que se diferencian entre:

|`/`| Raiz de base o Principio del sistema que contiene los archivos necesarios para arrancar el sistema operativo.|
|`/bin`| Contiene comandos de binarios escenciales  |
|`/boot`| Consiste en el estandar Bootloader, kernel ejecutable y los archivos necesario para arrancar el sistema operativo de Linux. |
|`/dev`| Contiene los archivos de un dispositivo para facilitar el acceso a todos dispositivos de hardware conectados al sistema. |
|`/etc`| Sistema local de archivos de configuración. Los archivos de configuración de las aplicaciones instaladas pueden ser guardados aquí. |
|`/home`| Cada usuario del sistema tiene su propio subdirectorio para el almacenamiento. |
|`/lib`| Archivos de las biblotecas compartidas que son necesarias para le arranque del sistema. |
|`/media`| Dispositivos externos de medios extraibles, como unidades USB se montan en este directorio. |
|`/mnt`| Punto de montaje temporal para regular los sistemas de fichos. |
|`/opt`| Directorio de archivos, tales como herraminetas de terceros. |
|`/root`| El directorio es del usuario *root* que es el que controla y permite el manejo del sistema. |
|`/sbin`| Este directorio contiene los archivos ejecutables que se utiliza para la administración del sistema. |
|`/tmp`| Se puede escribir, leer y ejecutar en este directorio de forma temporal. Este directorio es generalmente despejado en el arranque del sistema y pueden ser eliminados en otros momentos. sin ninguna advertencia. |
|`/usr`| Contiene los archivos ejecutables, biblotecas, manual de archivos, entre otros. |
|`var`| Este directorio contiene los archivos de datos variables tales como archivos de registro de correo electrónico, aplicaciones web relacionadas con los archivos, y otros más. |

