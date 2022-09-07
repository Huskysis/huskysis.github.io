---
layout: post
title:  "Fundamentos Básicos de Linux"
author: Husky
tags: [ Linux ]
image: assets/images/linux.png
---
### **Pequeño Resumen de ¿Qué es Linux?**

> Linux es un sistema operativo como Windows, iOS, Android o macOS. Un sistema operativo es un software que administra todos los recursos de hardware asociados con nuestra computadora. Eso significa que un sistema operativo gestiona toda la comunicación entre el software y el hardware. Además, existen muchas distribuciones diferentes (distro). Es como una versión de los sistemas operativos Windows.

> En general, Linux se considera más seguro que otros sistemas operativos y, si bien ha tenido muchas vulnerabilidades del kernel en el pasado, cada vez es menos frecuente. Es menos susceptible al malware que los sistemas operativos Windows y se actualiza con mucha frecuencia. Linux también es muy estable y, en general, ofrece un rendimiento muy alto para el usuario final. Sin embargo, puede ser más difícil para los principiantes y no tiene tantos controladores de hardware como Windows. 

* * *

### **Representasión de Envío, Entrada y Salida**

Los comandos en Linux tienen una entrada estándar (stdin 0) y dos salidas estándar: una para la salida normal del comando (stdout 1) y otra para la salida de los mensajes de error (stderr 2), por defecto la entrada como las salidas se muestran en la terminal del usuario, sin embargo al usar un operador de redirección podemos lograr que en lugar de usar la terminal se utilice un fichero. Puedes ver como ocurre [aqui]({{ "/Script-en-Bash" | prepend: site.baseurl | replace: '//', '/' }}).

* * *
### **Filosofía de Linux**

  Linux sigue cinco principios básicos:

| **Principios** | **Descripción**|
| -------------|:-------------- |
| **Todo es un archivos**| Todos los archivos de configuración para los diversos servicios que se ejecutan en Linux se almacenan en uno o más archivos de texto. |
| **Programas pequeños y de un solo proposito** | Muchas herramientas se pueden combinar para que trabajen juntas. |
| **Evita las interfaz de usuario cautivas** | La integración y combinación de diferentes herramientas nos permite llevar a cabo muchas tareas grandes y complejas, como procesar o filtrar resultados de datos específicos. |
| **Los datos de configuración se almacenan en un archivo de texto** | Un claro ejemplo es el archivo ubicado en **/etc/passwd** que tiene todos los usuarios registrados en el equipo. |
| **Capacidad de encadenar programas para realizar tareas complejas** | La integración y combinación de diferentes herramientas permite llevar a cabo muchas tareas grandes y complejas, como procesar o filtrar resultados de datos específicos. |

* * *

### **Arquitectura de Linux**

El **Sistema Operativo(OS)** de Linux se puede dividir en ciertas "*capas*":

| **Estructura**     | **Descripción**  |
| **Hardware**       | Dispositivo periféricos, tales como la memoria RAM del sistema, dico duro, CPU, entre otros. |
| **Kernel**         | El núcleo del sistema operativo Linux, cuya funciones es virtualizar y controlar los recursos comunes de Hardware, como la memoria asignada, acceso a datos, y otros. El kernel da a cada proceso con sus propios recursos virtuales y evita/mitiga los conflictos entre los diferentes procesos.|
| **Terminal(Shell)**| Es una interfaz de linea de comandos(CLI, command-line interface), también conocido como **SHELL**, es una terminal que se puede introducir comandos a ejecutar. |
| **Utilidades del Sistema** | El usuario tiene a disposición todas las funcionalidades del sistema operativo. |


* * *

### **Terminal o Shell**

> Una terminal de Linux, también llamada shell o línea de comandos, proporciona una interfaz de entrada/salida basada en texto entre los usuarios y el kernel para un sistema informático. El término consola también es típico pero no se refiere a una ventana sino a una pantalla en modo texto. En la ventana de terminal, se pueden ejecutar comandos para controlar el sistema.

> La Terminal o Shell más utilizado en Linux es el [Bourne-Again Shell(BASH)]({{ "/Comandos-de-Bash" | prepend: site.baseurl | replace: '//', '/' }}) y es parte del proyecto GNU. Todo lo que hacemos a través de la GUI lo podemos hacer con el shell. El shell nos brinda muchas más posibilidades de interactuar con programas y procesos para obtener información más rápido. Además, muchos procesos se pueden automatizar fácilmente con scripts más pequeños o más grandes que facilitan mucho el trabajo manual. 
* * *

### **Jerarquía del sistema de archivos**
 
La Estructura del *sistema de archivo* o *Raiz* es una Jerarquía de tipo árbol, que se diferencian entre:

|`.`| En cualquier Carpeta existira '.' como indicador del directorio actual.
|`..`| Indica el directorio anterior al actual.
|`/`| Raiz de base o Principio del sistema que contiene los archivos necesarios para arrancar el sistema operativo.|
|`/bin`| Contiene comandos de binarios escenciales  |
|`/boot`| Consiste en el estandar Bootloader, kernel ejecutable y los archivos necesario para arrancar el sistema operativo de Linux. |
|`/dev`| Contiene los archivos de un dispositivo para facilitar el acceso a todos dispositivos de hardware conectados al sistema. |
|`/etc`| Sistema local de archivos de configuración. Los archivos de configuración de las aplicaciones instaladas pueden ser guardados aquí. |
|`/home`| Cada usuario del sistema tiene su propio subdirectorio para el almacenamiento. |
|`/lib`| Archivos de las biblotecas compartidas que son necesarias para le arranque del sistema. |
|`/media`| Dispositivos externos de medios extraibles, como unidades USB se montan en este directorio. |
|`/mnt`| Punto de montaje temporal para regular los sistemas de fichos. |
|`/opt`| Directorio de archivos, tales como herramientas de terceros. |
|`/root`| El directorio es del usuario *root* que es el que controla y permite el manejo del sistema. |
|`/sbin`| Este directorio contiene los archivos ejecutables que se utiliza para la administración del sistema. |
|`/tmp`| Se puede escribir, leer y ejecutar en este directorio de forma temporal. Este directorio es generalmente despejado en el arranque del sistema y pueden ser eliminados en otros momentos. sin ninguna advertencia. |
|`/usr`| Contiene los archivos ejecutables, biblotecas, manual de archivos, entre otros. |
|`/var`| Este directorio contiene los archivos de datos variables tales como archivos de registro de correo electrónico, aplicaciones web relacionadas con los archivos, y otros más. |


