## Diseño de Sistemas Informáticos
## Informe Práctica 2: Instalación y configuración de Visual Studio Code

En esta práctica nos centraremos en la instalación y configuración del entorno de desarrollo [Visual Studio Code](https://es.wikipedia.org/wiki/Visual_Studio_Code) (VSC). Se trata de un editor de código con soporte a operaciones de desarrollo tales como debugging o control de versiones con Git, además puede porporcionar herramientas útiles para los programadores como resaltado de sintaxis o finalización inteligente de código que facilitan mucho la implementación de estos.

### --> Primeros pasos

Continuando con lo expuesto en el [informe anterior](https://ostream07.github.io/ull-esit-inf-dsi-20-21-github-campus-experts-ostream07/), veremos a continuación como instalarlo y configurarlos para poder hacer de VCS nuestro entorno de trabajo a lo laro de la asignatura.

Lo primero será instalarlo en nuestra máquina local el siguiente comando:
```
saul@DESKTOP-F06QRH9:~$ sudo apt install code
[sudo] contraseña para usuario: 
Leyendo lista de paquetes... Hecho
Creando árbol de dependencias       
Leyendo la información de estado... Hecho

No hay un paquete apt "code", pero hay un snap con ese nombre.
Intente «snap install code»

E: No se ha podido localizar el paquete code
```
Como se puede apreciar, nos hemos topado con un pequeño inconveniente, pero podemos solucionarlo probando con este otro comando:
```
saul@DESKTOP-F06QRH9:~$ sudo snap install code --classic
2021-02-24T09:53:44Z INFO Waiting for automatic snapd restart...
Se ha instalado code 622cb03f por Visual Studio Code (vscode✓)
```

### Configuración de VSC para conectarse a una máquina remota por SSH

Lo siguiente será conseguir realizar una conexión remota por `ssh` entre VSC y nuestra máquina virtual. Para ello primeramente, debemos establecer una conexión hacia nuestra máquina virtual, antes de nada, lo primero es aseguarnos de qu estamos conectados a la VPN de la ULL.

```
saul@DESKTOP-F06QRH9:~$ ssh usuario@10.6.129.123
usuario@10.6.129.123's password: 
```

Una vez dentro, abrimos en local una ventana de VSC. Si pulsamos `F1` y tecleamos `ssh`, vemos que no nos aparece la opción `SSH-Remote: Connect to host`, por ello nos dirigiremos hacia la sección de extensiones y tendremos que instalar la  extensión [SSH-Remote](https://marketplace.visualstudio.com/items?itemName=ms-vscode-remote.remote-ssh) que nos permitirá usar una máquina remota con un **servidor SSH** como entorno de desarrollo.

Una vez instalada, podemos volver continuar. Pulsamos `F1` y volvemos a teclear `ssh`, esta vez entre las opciones vemos como si que aparece `SSH-Remote: Connect to host`, pinchamos y se nos despliega un pequeño menú donde puede aparecer el nombre del host de máquinas que ya tengamos asignadas si es que hemos utilizado esta herramienta con anterioridad y además otras como `Add New SSH Host...` y `Configure SSH Hosts...`.
Si la configuración realizada en el informe anterior sobre el fichero `~/.ssh/config` fue realizada con éxito, debería de aparecer el nombre del host directamente, de no ser el caso, como me ocurre a mí, podemos acceder a `Configure SSH Hosts...` y desde aquí movernos hacia `(AÑADIR RUTA).
```
Host DSI-30
  HostName DSI-30
  User usuario
```
Guardamos, y ahora si volvemos a seguir con los pasos descritos anteriormente, al acceder a `SSH-Remote: Connect to host` podemos ver como sale el nombre de nuestro host, en mi caso, `DSI-30`.







