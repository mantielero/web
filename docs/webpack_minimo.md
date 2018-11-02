# Ejemplo mínimo
## Introducción
Basado en [minimal webpack html](https://github.com/knpwrs/minimal-webpack-html) y en [webpack by example - part 1](https://medium.com/front-end-hacking/webpack-by-example-part-1-1d07bc42006a).

## Proyecto
Generamos tres ficheros:

- webpack.config.js: contiene la configuración de webpack. Básicamente define cuál es el primer fichero a leer y dónde escribe la salida del proceso de bundling.
- index.html: lo dejamos directamente en el directorio de distribución. Referencia al fichero *bundle.js* que es el resultado del bundling.
- index.js: es el fichero original (antes del bundling). Básicamente escribe en la consola del navegador.

```js tab="webpack.config.js"
//var webpack = require('webpack');
var path = require('path');

var BUILD_DIR = path.resolve(__dirname, 'src/client/public');
var APP_DIR = path.resolve(__dirname, 'src/client/app');

var config = {
  entry: APP_DIR + '/index.js',
  output: {
    path: BUILD_DIR,
    filename: 'bundle.js'
  }
};

module.exports = config;
```

```html tab="./src/client/public/index.html"
<!doctype html>
<html lang="en">
  <head>
    <meta charset="utf-8">
    <meta name="viewport" content="width=device-width, initial-scale=1">
    <title>Webpack - ejemplo minimo</title>
  </head>
  <body>
    <script src="bundle.js"></script>
  </body>
</html>
```

```js tab="./src/client/app/index.js"
window.console.log('Hola mundo');
```

en donde:

- Por una lado se requieren las librerías *webpack* y *path*
- La librería *path* sirve para componer las rutas de *BUILD_DIR* y *APP_DIR*. Vemos que la aplicación cliente se guarda en *src/client/app*.
- En la variable *config* e identifica el punto de entrada *entry* y la salida *output* (se indica la ruta y el nombre del fichero *bundle.js*).


## Construcción
Bastará con ejecutar:
```bash
$ node_modules/.bin/webpack -p
Hash: 4715fed529234f0c574b
Version: webpack 4.23.1
Time: 85ms
Built at: 02/11/2018 16:11:43
    Asset       Size  Chunks             Chunk Names
bundle.js  963 bytes       0  [emitted]  main
Entrypoint main = bundle.js
[0] ./src/client/app/index.js 83 bytes {0} [built]
```
que ejecuta *webpack* en el modo para producción.

## Ejecución
Visitaremos:
```
$ firefox src/app/public/index.html
```

y abrimos la consola del navegador pulsando Ctrl+Shift+K
