# Aplicaciones de Escritorio

## Introducción


Nos basaremos en [este tutorial](https://scotch.io/tutorials/creating-desktop-applications-with-angularjs-and-github-electron#creating-your-electron-application) y en la documentación de [Electron](https://github.com/atom/electron/tree/v0.36.7/docs).

Hello world
-----------

Creamos un directorio:
```
$ mkdir electron01
$ cd electron01
```

La estructura básica de una aplicación basada en Electron, requiere: package.json, index.html, main.js.

Y generamos un "package.json" básico mediante::
```
$ npm init
```

o directamente a mano::
```js
  {
    "name": "electron01",
    "version": "0.0.1",
    "main": "app/main.js"
  }
```

En donde, como vemos, indicamos donde reside el ejecutable desde el que se arranca la aplicación.

Instalamos el ejecutable precompilado que ejecuta aplicaciones "electron":
```
  $ npm install --save electron-prebuilt
```

Creamos dentro de "./app" un fichero "main.js" básico:
```
  $ mkdir app
  $ touch app/main.js
```

con la siguiente pinta:
```js

  // app/main.js

  // Module to control application life.
  var app = require('app');

  // Module to create native browser window.
  var BrowserWindow = require('browser-window');
  var mainWindow = null;

  // Quit when all windows are closed.
  app.on('window-all-closed', function () {
   if (process.platform != 'darwin') {
     app.quit();
   }
  });

  // This method will be called when Electron has finished
  // initialization and is ready to create browser windows.
  app.on('ready', function () {

   // Create the browser window.
   mainWindow = new BrowserWindow({ width: 800, height: 600 });

   // and load the index.html of the app.
   mainWindow.loadUrl('file://' + __dirname + '/index.html');

   // Open the devtools.
   // mainWindow.openDevTools();
   // Emitted when the window is closed.
   mainWindow.on('closed', function () {

     // Dereference the window object, usually you would store windows
     // in an array if your app supports multi windows, this is the time
     // when you should delete the corresponding element.
     mainWindow = null;
   });

  });
```

Vemos que este fichero referencia el fichero "index.html". La variable de node [__dirname](https://nodejs.org/docs/latest/api/globals.html#globals_dirname) contiene el directorio en el que se encuentra el fichero "main.js".

Creamos un fichero "index.html" básico que será el que vaya embebido en la aplicación:
```html
  <html>
  <body>
    <h1>Hello World!</h1>
     We are using Electron
     <script> document.write(process.versions['electron']) </script> on
     <script> document.write(process.platform) </script>
     <script type="text/javascript">
        var fs = require('fs');
        var file = fs.readFileSync('app/package.json');
        document.write(file);
     </script>

  </body>
  </html>
```

Ejecutamos la aplicación local::
```
$ node_modules/.bin/electron ./
```

aquí referenciamos la carpeta en la que tenemos "package.json".

## Automatizar tarea

Para automatizar la ejecución usaremos la herramienta "gulp". Si no la tenemos, la instalamos como parte de nuestro desarrollo. Aunque ésta es deseable tenerla globalmente:
```
  # npm install -g gulp
```

En nuestro proyecto, tendremos que editar el fichero **gulpfile.js** para que tenga una pinta como la siguiente:
```js

  // get the dependencies
  var gulp        = require('gulp'),
    childProcess  = require('child_process'),
    electron      = require('electron-prebuilt');

  // create the gulp task
  gulp.task('run', function () {
    childProcess.spawn(electron, ['./'], { stdio: 'inherit' });
  });
```

Ahora ejecutamos nuestro proyecto mediante::
```
  $ gulp run
```

Si tenemos instalado en Atom el paquete "gulp-control", podemos ejecutar las tareas mediante "C-A-O".

### package.json


En este ejemplo, tenemos un único "package.json". En otros sitios mantienen dos ficheros: uno dentro de la aplicación y otro en el raíz.


## Interfaz Gráfica

Como vemos, Electron no fuerza que se use ningún toolkit concreto. Podemos usar bootstrap, polymer, angular-material, jquery, dojo, material design lite, ... Tampoco hay nada que nos impida usar Angular2, Vue, ... o cualquier otro framework.

Por ejemplo haremos un formulario sencillito basado en Polymer.

### Material Design Lite

Parece suficiente para aplicaciones relativamente sencillas. Pero pronto se queda un poco corto.

En el <head> de index.html, añadimos::
```html
  <link rel="stylesheet" href="http://fonts.googleapis.com/icon?family=Material+Icons">
  <link href='http://fonts.googleapis.com/css?family=Roboto:400,300,300italic,500,400italic,700,700italic' rel='stylesheet' type='text/css'>
  <link rel="stylesheet" href="http://storage.googleapis.com/code.getmdl.io/1.0.1/material.teal-red.min.css" />
```

Pero para no depender de la disponibilidad de conexión de internet::
```
  $ bower install material-design-lite --save
```

De esta forma, las referencias serán de la forma::
```html
  <link rel="stylesheet" href="../bower_components/material-design-lite/material.min.css">
  <script src="../bower_components/material-design-lite/material.min.js"></script>
  <link rel="stylesheet" href="https://fonts.googleapis.com/icon?family=Material+Icons">
```

Después podemos añadir componentes basados en la [documentación](https://www.getmdl.io/).

Aquí tenemos un pequeño [tutorial](http://webdesign.tutsplus.com/es/tutorials/learning-material-design-lite-cards--cms-24633) sobre tarjetas.


### Polymer

Los paquetes del lado cliente los instalamos con "bower". Por ello inicializamos mediante:
```
  $ bower init
```

y ejecutamos:
```
  $ bower install --save Polymer/polymer
```

https://scotch.io/tutorials/build-a-real-time-polymer-to-do-app
