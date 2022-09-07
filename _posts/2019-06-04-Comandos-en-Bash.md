---
layout: post
title:  "Comandos en Bash"
author: Husky
categories: [ Bash ]
tags: [ Linux ]
image: assets/images/bash.png
---
### **Pequeño resumen de Bash**

> El nombre **Bash** (del Inglés *Bourne-Again-SHell*) es un tipo de Terminal que interpreta líneas de comando que lée. ejecuta e interpreta de forma estandar la salida de la información del archivo o el conjunto que se vaya a programar.

> Bash es una herramienta versátil para realizar diversas tareas, lo que en algunos casos le permite evitar la instalación de software especializado. Al mismo tiempo, es un lenguaje de programación de secuencias de comandos que le permite crear cadenas de comandos para automatizar varias operaciones. 


```
Todo sera mi interpretación de Bash.  Se puede informar más escribiendo en su terminal " man bash ".
```

--- 
 
### **OPCIONES**  
  
  Todas las siguientes Abreviaturas son acompañadas por su descripción y un ejemplo.  
  Para invocar desde la Terminal la **Bash** se debe escribir `Bash`.  
  En adición las siguientes *Opciones* son interpretadas especificando el prefijo `Bash ..` seguido con las siguientes opciones:  
  
| `-c` | Este Comando es interpretado de forma que el argumento que no sea una opción sea una cadena comando. Me explico, usando `bash -c $comando` es invocar *Bash* seguido de ejecutar el comando en **$comando**.  Ejemplo: `bash -c ls` invoca *Bash* y sigue con el comando *ls* que lista lo que haya en la ruta actual. |
| `-i` | Entra en modo interactivo en la Terminal. Ejemplo: `bash -i`  \**invoca la terminal bash*\*.|
| `-l` | Es como un metodo de inicio seción pero con una Terminal. Ejemplo: `bash -l` \**entra en una sesión como un usuario*\*. |
| `-r` | Invoca la Terminal de forma **Limitada** y lo ejecuta en un ambiente controlado. Ejemplo `bash -r` *solo se tendra acceso a ciertos comandos*. |
| `-s` | La Terminal se inicia de forma Estandar si no se le adjunto ningún argumento. Ejemplo `bash -s` \**se inicio la terminal*\*. |

---

### **COMANDOS DE AYUDA**

  Los siguientes Comandos se pueden ejecutar acompañados o solos(tambíen se le puede adjuntar el parametro "--help o -h" para ver la ayuda o guia del programa):

|`man`| Muestra la utilidad/es, instrucciones y su uso del comando a solicitar. Ejemplo: `man bash` *(no todos disponen del mismo)*. |
|`info`| Da información del comando indicado. Ejemplo: `info ls`. |
|`help`| Se puede ejecutar en casi cualquier ámbito, provee algunos comandos a ejecutar. |
|`whatis`| Provee una breve descripción de los comandos. Ejemplo `whatis mkdir`. |

---  

### **COMANDOS PARA ARCHIVOS O DIRECTORIOS**

  Los siguientes Comandos se ejecutan en cualquier ámbito desde una Terminal:

|`echo`| Reporta en la Terminal lo que escribias |
|`ls`| Lista el contenido actual, o si le asignas una dirección te lo muestra. Ejemplo: `ls /home` , lista el contenido de la carpeta /home. |
|`cd`| En una Terminal, te mueves entre directorios. Ejemplo: `cd ~/Desktop` , te mueves al Escritorio propio. |
|`pwd`| Muestra la ruta actual en la que te ubicas. |
|`tree`| Muesrta en forma de ramificación las rutas disponibles y sus archivos. |
|`mkdir`| Crea un directorio seguido de su nombre. Ejemplo: `mkdir "Nueva Carpeta"` |
|`rmdir`| Borra un directorio(este debe de estar vacio). Ejemplo: `rmdir "Nueva Carpeta"` |
|`touch`| Crea un archivo. Ejemplo: `touch archivo.txt` |
|`cp`| Copia un archivo asignandole el archivo y hacia donde pega la copia. Ejemplo: `cp archivo.txt ~/Desktop` , copia el *archivo.txt* y lo pega en el escritorio. |
|`file`| Te muestra los detalles del archivo. |
|`cat`| Leé y muestra el contenido del archivo asignado. Ejemplo: `cat archivo.txt` |
|`mv`| Mueve el archivo asignado hacia una ubicación. Ejemplo: `mv archivo.txt ~/Download` , muevo el *archivo.txt* a Descargas. |
|`rm`| Elimina el archivo o directorio asignado. Ejemplo: `rm archivo.txt` , en caso del directorio  `rm -d` |
|`shred`| Elimina el archivo de forma casi permanente. |
|`sort`| Muestra de forma ordenada alfabeticamente el contenido de un archivo asignado. |
|`more`| Leé y lo muestra en formato Página a Página el archivo asignado. |
|`less`| Leé y muestra el contenido del archio desde el inicio. |
|`split`| Divide el archivo y le asigna *aa* al final. Ejemplo: `split archivo.txt` se crea el mismo archivo pero con el nombre "xaa" |
|`find`| Busca archivos asignados. Ejemplo: `find archivo.txt` localiza el archivo y te muestra su dirección. |
|`locate`| Busca directorios asignados y muestra su contenido. Ejemplo: `locate ~` busca en el directorio de usuarios. |
|`updatedb`| Actualiza la lista de archivo o programas instalados. |
|`whereis`| Muestra la ruta de un archivo en especifico. Ejemplo: `whereis archivo.txt`. |
|`wc`| Muestra el número total de líneas, palabras y caracteres que contiene el archivo asignado. |
|`grep`| Busca por el texto que se le indique acompañado de un archivo. Ejemplo: `grep 'root' /etc/passwd`, en ese archivo busca la palabra *root*. |
|`head`| Muestra las primeras 10 líneas que contiene el archivo. |
|`tail`| Muestra las ultimas 10 líneas que contiene el archivo |
|`tr`| Reemplaza caracteres que se ven en el archivo. Ejemplo: `cat archivo.txt | tr -d "algo"` quita "algo" que muestre en consola el archivo, su contenido no se altera. |
|`ln`| Crea un enlace entre de un archivo. Ejemplo: `ln archio.txt archivo2.txt`. |
|`diff`| Muestra diferencias entre dos archivo. Ejemplo: `diff archivo.txt archivo2.txt` el primer archivo se representa como "<" y el segundo con ">". |
|`chmod`| Permite darle o quitarle los *permisos* que se puede hacer o no de un archivo o directorio. Ejemplo: `chmod +x archivo.txt` , el *archivo.txt* tiene permiso de ejecución. |
|`chown`| Permite cambiar de propietario el archivo o directorio. Ejemplo: `chown usuario:usuario archivo.txt` , el *usuario* es propietario del archivo.txt. |
|`chgrp`| Cambia de grupos del usuario. Ejemplo: `chgrp grupo archivo.txt`.  |
|`vi`| Abre un editor de texto. |
|`nano`| Abre un editor de texto |

---

### **COMANDOS DE GESTIÓN DE PROCESOS**

|`top`| Muestra todos los procesos que se estan ejecutando. |
|`ps`| Muestra los procesos actuales. |
|`kill`| Termina un proceso por su PID(Control Proporcional Integral Derivativo), se puede ver mediante el comando `top`. |
|`time`| Mide el tiempo que tarda el proceso en ejecutarse. |
|`bg`| Pone en segunda plano el proceso. |
|`fg`| Trae a primer plano un proceso detenido en segundo plano. |
|`&`| Colocado al final de la línea de un comando,lo ejecuta en segundo plano. |

---

### **COMANDOS PARA GESTIÓN DEL SISTEMA**

|`exit`| Cierra la Terminal. |
|`logout`| Cierra la sesión de la Terminal. |
|`history`| Muestra los comandos ejecutados por el usuario. |
|`uname`| Da información del sistema operativo. |
|`tee`| Copia la entrada estándar a la salida estándar a un archivo. |
|`hostname`| Muestra el nombre del servidor. |
|`date`| Muestra la fecha y hora actual. |
|`clear`| Borra las líneas de texto que quedaron escritas en la Terminal. |
|`env`| Muestra las variables del entorno. |
|`export`| Permite el uso de variables por programas en todos los caminos del usuario. |
|`uptime`| Muestra el tiempo transcurrido desde que se encendio la máquina. |

---

### **COMANDOS PARA LA GESTIÓN DE USUARIOS**

|`adduser / useradd`| Agrega un nuevo usuario. |
|`userdel`| Borra un usuario. |
|`passwd`| Cambia la contraseña. |
|`whoami`| Muestra el nombre de usuario acutal. |
|`id`| Muestra indentificadores del usuario. |
|`w`| Muestra detalles de los usuarios actuales. |
|`addgroup / groupadd`| Agrega un nuevo grupo. |

---

### **COMANDOS PARA ACCESO REMOTO**

|`ssh`| Permite conectarte de forma remota a otra máquina mediante una Terminal. |
|`ftp`| Permite conectarte mediante el protocolo FTP(File Transfer Protocol, o Protocolo de Transferencia de Archivos). |
|`telnet`| Permite la conexión a un equipo o máquina de forma remota. |

---

### **COMANDOS DE RED**

|`ping`| Indica si hay respuesta por parte del servidor. |
|`ifconfig`| Muestra la configuración del dispositivo de red. |
|`nmap`| Es una herramienta que escanea la red y muestra los puertos que se encuentran disponibles, la descripción y su versión del mismo entre otras funciones. |

---

### **COMANDOS PARA APAGAR O REINICIAR EL SISTEMA**

|`shutdown / halt / poweroff`| Apaga el sistema. |
|`reboot`| Reinicia el sistema. |

---

Sí entendiste todo, vamos a la sección de [Scripting en Bash](../Script-en-Bash)

