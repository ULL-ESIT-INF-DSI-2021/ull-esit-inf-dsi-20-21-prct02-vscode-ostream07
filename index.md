## Diseño de Sistemas Informáticos
## Informe Práctica 2: Instalación y configuración de Visual Studio Code

En esta práctica nos centraremos en la instalación y configuración del entorno de desarrollo [Visual Studio Code](https://es.wikipedia.org/wiki/Visual_Studio_Code) (VSC). Se trata de un editor de código con soporte a operaciones de desarrollo tales como debugging o control de versiones con Git, además puede porporcionar herramientas útiles para los programadores como resaltado de sintaxis o finalización inteligente de código que facilitan mucho la implementación de estos.

### --> Primeros pasos

Continuando con lo expuesto en el [informe anterior](https://ostream07.github.io/ull-esit-inf-dsi-20-21-github-campus-experts-ostream07/), veremos a continuación como instalarlo y configurarlos para poder hacer de VCS nuestro entorno de trabajo a lo largo de la asignatura.

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

### --> Configuración de VSC para conectarse a una máquina remota por SSH

Lo siguiente será conseguir realizar una conexión remota por `ssh` entre VSC y nuestra máquina virtual. Para ello primeramente, debemos establecer una conexión hacia nuestra máquina virtual, antes de nada, lo primero es aseguarnos de qu estamos conectados a la VPN de la ULL.

```
saul@DESKTOP-F06QRH9:~$ ssh usuario@10.6.129.123
usuario@10.6.129.123's password: 
```

Una vez dentro, abrimos en local una ventana de VSC. Si pulsamos `F1` y tecleamos `ssh`, vemos que no nos aparece la opción `SSH-Remote: Connect to host`, por ello nos dirigiremos hacia la sección de extensiones y tendremos que instalar la  extensión [SSH-Remote](https://marketplace.visualstudio.com/items?itemName=ms-vscode-remote.remote-ssh) que nos permitirá usar una máquina remota con un **servidor SSH** como entorno de desarrollo.

Una vez instalada, podemos volver continuar. Pulsamos `F1` y volvemos a teclear `ssh`, esta vez entre las opciones vemos como si que aparece `SSH-Remote: Connect to host`, pinchamos y se nos despliega un pequeño menú donde puede aparecer el nombre del host de máquinas que ya tengamos asignadas si es que hemos utilizado esta herramienta con anterioridad y además otras como `Add New SSH Host...` y `Configure SSH Hosts...`.
Si la configuración realizada en el informe anterior sobre el fichero `~/.ssh/config` fue realizada con éxito, debería de aparecer el nombre del host directamente, de no ser el caso, como me ocurre a mí, podemos acceder a `Configure SSH Hosts...` y desde aquí movernos hacia `~/.ssh/config`.
```
Host DSI-30
  HostName DSI-30
  User usuario
```
Guardamos, y ahora si volvemos a seguir con los pasos descritos anteriormente, al acceder a `SSH-Remote: Connect to host` podemos ver como sale el nombre de nuestro host, en mi caso, `DSI-30`.


### --> Paquetes de instalación de interés

Una de las muchas extensiones que tiene VSC es [Live Share Extension Pack](https://marketplace.visualstudio.com/items?itemName=MS-vsliveshare.vsliveshare-pack) que nos permite realizar sesiones colaborativas con otros usuarios en tiempo real.

Para el desarrollo de las próximas prácticas, será conveniente tener el editor `Vi/Vim` así como la extensión [ESlint](https://marketplace.visualstudio.com/items?itemName=dbaeumer.vscode-eslint). Si no tenemos instalado ESLint de forma local o global, podemos hacerlo ejecutando `npm install eslinten` sobre la carpeta del área de trabajo para una instalación local o podemos ejecutar `npm install -g eslint` para una instalación global.

```
[~/practica2/ull-esit-inf-dsi-20-21-prct02-vscode-ostream07(desarrollo)]$npm install -g eslint

added 110 packages, and audited 111 packages in 11s

13 packages are looking for funding
  run `npm fund` for details

found 0 vulnerabilities
npm notice 
npm notice New patch version of npm available! 7.5.3 -> 7.5.6
npm notice Changelog: https://github.com/npm/cli/releases/tag/v7.5.6
npm notice Run npm install -g npm@7.5.6 to update!
npm notice 
```

### --> Primer proyecto en TypeScript: “Hola Mundo”

A continuación, lo que haremos será instalar el compilador de TypeScript. Al igual que en el caso anterior, la opción **--global** nos permitirá instalar el paquete de forma global. Para ello, usaremos npm:
```
[~/practica2/ull-esit-inf-dsi-20-21-prct02-vscode-ostream07(desarrollo)]$npm install --global typescript

added 1 package, and audited 2 packages in 4s

found 0 vulnerabilities
```

Ahora vamos a crear un directorio en el que desarrollaremos nuestro proyecto con `mkdir`, y dentro de él pasaremos a establecer las dependencias de desarrollo y ejecución del proyecto a modo de paquetes de los que va a depender el proyecto actual, para ello requeriremos de un fichero denominado `package.json`. Para lograrlo, necesitaremos utilizar el comando `npm init --yes`:
```
[~/practica2/ull-esit-inf-dsi-20-21-prct02-vscode-ostream07(desarrollo)]$mkdir hello_world
[~/practica2/ull-esit-inf-dsi-20-21-prct02-vscode-ostream07(desarrollo)]$cd hello_world/
[~/practica2/ull-esit-inf-dsi-20-21-prct02-vscode-ostream07/hello_world(desarrollo)]$npm init --yes
Wrote to /home/usuario/practica2/ull-esit-inf-dsi-20-21-prct02-vscode-ostream07/hello_world/package.json:

{
  "name": "hello_world",
  "version": "1.0.0",
  "description": "",
  "main": "index.js",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1"
  },
  "keywords": [],
  "author": "",
  "license": "ISC"
}
```
Si queremos comprobar que todo ha salido con éxito, podemos comprobarlo mediante un `ls -ll`, o también podríamos ejecutar `ls -lrtha` para tener una información más detallada: 

* **-l** --> muestra un listado en el formato largo, con información de permisos, usuario, grupo ...
* **-r** --> se invierte el orden, mostrando los archivos más recientes al final.
* **-t** --> muestra ordenado por la fecha de última modificación.
* **-h** --> con -l imprime el tamaño de los archivos de forma entendible para los humanos (ej. 1K 234M 2G).
* **-a** --> muestra los archivos ocultos.

```
[~/practica2/ull-esit-inf-dsi-20-21-prct02-vscode-ostream07/hello_world(desarrollo)]$ls -ll
total 4
-rw-rw-r-- 1 usuario usuario 225 feb 25 17:47 package.json
```
```
[~/practica2/ull-esit-inf-dsi-20-21-prct02-vscode-ostream07/hello_world(desarrollo)]$ls -lrtha
total 12K
drwxrwxr-x 4 usuario usuario 4,0K feb 25 17:47 ..
drwxrwxr-x 2 usuario usuario 4,0K feb 25 17:47 .
-rw-rw-r-- 1 usuario usuario  225 feb 25 17:47 package.json
```
Para más información sobre el comando `ls` y sus opciones puede consultar el manual con `man ls`, tamién hay varios recursos que podemos consultar por internet como [este](https://es.wikipedia.org/wiki/Ls).

Dentro de nuestro directorio `hello_world` vamos a crear un nuevo fichero llamado **tsconfig.json** al que le añadiremos en su interior las siguientes líneas:
```
{
  "compilerOptions": {
    "target": "ES2018",
    "outDir": "./dist",
    "rootDir": "./src",
    "module": "CommonJS"
  }
}
```

Con estas opciones de configuración queremos indicarle al compilador de TypeScript una serie de datos:

1. Queremos generar código compatible con el estándar **ES2018** de JavaScript. 
2. El código JavaScript producto de la compilación se almacenará en un directorio `/dist`. 
3. Especificamos que el código fuente escrito en TypeScript se encuentra en el directorio `src`.
4. Indicamos un estándar para cargar código desde ficheros independientes.

Creamos el directorio `src` y también un fichero llamado `index.ts`
```
[~/practica2/ull-esit-inf-dsi-20-21-prct02-vscode-ostream07/hello_world(desarrollo)]$mkdir src
[~/practica2/ull-esit-inf-dsi-20-21-prct02-vscode-ostream07/hello_world(desarrollo)]$ls
package.json  src  tsconfi.json
[~/practica2/ull-esit-inf-dsi-20-21-prct02-vscode-ostream07/hello_world(desarrollo)]$cd src/
[~/practica2/ull-esit-inf-dsi-20-21-prct02-vscode-ostream07/hello_world/src(desarrollo)]$touch index.ts
```
A continuación vamos a realizar un *hola mundo*, para ello primero debemos escribir las siguientes líneas de código en el fichero que acabamos de crear, guardar los cambios y acto seguido, ejecutar con `tsc`.
```
let myString: string = "Hola Mundo";
console.log(myString);
```

En mi caso, en la primera palabra de la priemra línea, **let**, aparece subrayada y con el siguiente mensaje eal situal el ratón sobre ella.

`ESLint is disabled since its execution has not been approved or denied yet. Use the light bulb menu to open the approval dialog.eslint`

En la esquina inferior derecha de el VSC, en la banda azul, aparece **ESLINT** con un signo de prohibido, pulsamos y seleccionamos **Allow**.

Ahora si, podemos ejecutar 

Vemos que se crea automáticamente el directorio `dist` y dentro de él un fichero llamado `index.js`. Vamos a compararlos para ver si hay diferencias entre ellos:
```
[~/practica2/ull-esit-inf-dsi-20-21-prct02-vscode-ostream07/hello_world(desarrollo)]$diff src/index.ts dist/index.js 
1,2c1,2
< let myString: string = "Hola Mundo";
< console.log(myString);
\ No hay ningún carácter de nueva línea al final del archivo
---
> let myString = "Hola Mundo";
> console.log(myString);
```

En teoría, solo tendría que haber salido una, la declaración de la variable **myString**, pero en mi fichero `index.js`, había una tercera línea vacía, al eliminarla y volver a ejecutar, todo sale satisfactoriamente:

```
[~/practica2/ull-esit-inf-dsi-20-21-prct02-vscode-ostream07/hello_world(desarrollo)]$diff src/index.ts dist/index.js 
1c1
< let myString: string = "Hola Mundo";
---
> let myString = "Hola Mundo";
```

Para finalizar, podemos probar el código JavaScript generado a partir del código TypeScript mediante el siguiente comando:

```

```
