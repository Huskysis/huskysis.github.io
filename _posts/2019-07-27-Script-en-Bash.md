---
layout: post
title:  "Script en Bash"
author: Husky
categories: [ Bash ]
image: assets/images/bash-1.png 
tags: [ Linux ]
---

## **Reconocimiento de Syntaxis y su Funcionalidad**

Primero es lo primero, saber como es una Syntaxis en Bash. Empezaremos con las báscias funcionalidad descriptivas para sabes donde se almacena las variables, los condicionales, entre otros.
Existen 

> Terminologías y Ejemplos:    
> > El Simbolo **$** representa el comando a ejecutar en la Terminal.

```Bash
 [User@Linux]$ echo "Hola y esto es una prueba."
  Hola y esto es una prueba.
```

> Para hacer que un nombre, comandos o algo relacionado te lo represente se usa las Variables, es **"algo"="esto"**(ahora la palabra **"$algo"** tiene almacenado **"esto"**), se interprete la variable almacenada con **$** seguido por su interprete "**algo**".

> Los Númerales **#** son comentarios y no se ejecutan en el proceso.

```Bash
# La Variable es "Nombre" y "Huskysis" es su contenido.
 [User@Linux]$ Nombre=Huskysis
 [User@Linux]$ echo $Nombre
  Huskysis

# Tambíen se le asigna un comando en "$( )"
 [User@Linux]$ echo $(comando)
 [User@Linux]$ echo $(pwd) Algo
  /home/usuario/Desktop Algo
```

> Para almacenar la salida redirigir el flujo del programa ya sea dependiendo de su estado final sea exitoso o erroneo se usa **>**(Mayor que) o **>>** para agregarlo al archivo sin sustituir todo el contenido, ya que **>** sustituye todo el conetnido por la salida del comando.

```Bash
# Se almacena integro al archivo.
 [User@Linux]$ echo "Prueba" > Archivo.txt
 [User@Linux]$ cat Archivo.txt
  Prueba

# Se coloca al final de la línea y asi con las siguientes entrantes.
 [User@Linux]$ echo "Otra Prueba" >> Archivo.txt
 [User@Linux]$ cat Archivo.txt
  Prueba
  Otra Prueba
```

> Para redirigir las salidas en su formato exitoso(el mensaje se envió y obtuvo una respuesta) o no exitoso(el mensaje se envió y no obtuvo una respuesta o un inconveniente al momento de reportarlo en la terminal o ejecutarlo) a un archivo se usa **1>**(stout 1) para la salida que da una respuesta exitosa, en cambio **2>**(stdrr 2) para la salida de una respuesta no exitosa.

```Bash
# Salida no exitosa
  [User@Linux]$ ls /root 
  ls: no se puede abrir el directorio '/root': Permiso denegado
  
  [User@Linux]$ ls /root 2> Error
  [User@Linux]$ cat Error
  ls: no se puede abrir el directorio '/root': Permiso denegado

# Salida exitosa
  [User@Linux]$ ls /home
  /usuario
  
  [User@Linux]$ ls /home 1> Normal
  [User@Linux]$ cat Normal
  /usuario

# Para la Salida Exitosa y No Existosa
  [User@Linux]$ls /root /home
  /home:
  usuario
  ls: no se puede abrir el directorio '/root': Permiso denegado

  [User@Linux]$ ls /root /home &> Ambos
  [User@Linux]$ cat Ambos
  /home:
  husky
  ls: no se puede abrir el directorio '/root': Permiso denegado
```

> Para hacer una secuencia se pueden lograr de muchas formas, pero me voy a limitar con usar el formato con **echo**.

```Bash
# Se utilizan las llaves "{ }" para indicar posible contenido. 
# Se utilizan los dos puntos ".." para indicar que sigua la secuencia.
 [User@Linux]$ echo {1..5}
  1 2 3 4 5

 [User@Linux]$ echo {a..f}
  a b c d f
  
 [User@Linux]$ echo {A..D}.txt
  A.txt B.txt C.txt D.txt
```
* * *  

## **Parametros Expansivos**

> Las siguientes representaciones serán dentro de un archivo.sh(la extención .sh da a enteder que es un archivo de comandos de Bash).

## **Bucle**
```bash
#!/usr/bin/env bash

# Bucle normal.
  for h in /etc/p*;do 
    echo $h
  done

# Bucle tipo C.
  for (( f = 0 ; f < 100 ; f++ ));do
    echo $f
  done

# Bucle rangos.
  for b in {1..5};do
    echo "Número $b"
  done

# Bucle rangos por tramos.
 for v in {0..50..5};do
   echo "Número $v"
  done

# Bucle leyendo líneas.
 cat file.txt | while read line;do
   echo $line
  done

# Bucle infinito.
 while true;do
   echo "Prueba"
  done
```

## **Funciones**

```sh
# Definiendo Funciones.
# La palabra "algo" sin el "=" queda como $1 y si hay cambia el orden del número con la variable.
  function algo() {
      echo "Esto $1"
    }
algo "es una prueba"

# Reportes de Errores. 
# Comando con salida exitosa (return 0).
# Comando con salida no exitoso (return 1).
  CualquierCosa(){
      return 1
    }
  if CualquierCosa; then
    echo "Correcto"
  else
    echo "Erroneo"
  fi
```

## **Condicionales**

```sh
# Tenga en cuenta que "[[" es en realidad un comando o programa que devuelve 0(verdadedo) o 1(falso).
 [[ -z CADENA ]]         # Cadena vacia.
 [[ -n CADENA ]]         # Cadena no vacia.
 [[ CADENA == CADENA ]]  # Son iguales.
 [[ CADENA != CADENA ]]  # No son iguales.
 [[ Núm -eq Núm ]]       # Son iguales.
 [[ Núm -ne Núm ]]       # No son iguales.
 [[ Núm -lt Núm ]]       # Menos que eso.
 [[ Núm -le Núm ]]       # Menos o igual a eso.
 [[ Núm -gt Núm ]]       # Mayor que eso.
 [[ Núm -ge Núm ]]       # Mayor o igual a eso.
 [[ CADENA =~ CADENA ]]  # Expresioes regulares(Regex).
 (( Núm < Núm ))         # Números condicionales.

# Archivos Condicionales
  [[ -e ARCHIVO ]]           # Existe.
  [[ -r ARCHIVO ]]           # Leible.
  [[ -h ARCHIVO ]]           # Enlace Simbolico.
  [[ -d ARCHIVO ]]           # Directorio.
  [[ -w ARCHIVO ]]           # Escribible.
  [[ -s ARCHIVO ]]           # Si el tamaño es >(mayor que) 0 bytes.
  [[ -f ARCHIVO ]]           # El archivo.
  [[ -x ARCHIVO ]]           # Ejecutable.
  [[ ARCHIVO1 -ef ARCHIOV2 ]]# Mismo archivo.
  [[ ARCHIVO1 -nt ARCHIVO2 ]]# El 1 es mas reciente que 2.
  [[ ARCHIVO1 -ot ARCHIVO2 ]]# El 2 es mas reciente que 1.

# Ejemplos
# La variable "$Caja" no esta asignada, por ende,"Esta vacio"
  if [[ -z $Caja ]]; then
     echo "Esta vacio"
   elif [[ -n $Caja ]]; then
     echo "No esta vacio"
   else
     echo "Error"
  fi

# Regex  
  if [[ "A" =~ ! ]]; then
    echo "Esto es Regex"
  elif [[ "A" =~ ! ]];then
    echo "Esto no es Regex"
  fi

# Números condicionales
  if (( 1 < 2));then
    echo "1 es menos que 2"
  elif (( 1 > 2 ));then
    echo "2 es mayor que 1"
  fi 

# Archivo
  if [[ -e "archivo.txt" ]];then
    echo "El archivo existe"
  else
    echo "El archivo no existe"
  fi
```

