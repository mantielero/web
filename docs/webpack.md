# Webpack

## Introducción
El código web es complejo. Usa HTML, CSS3 y javascript como mínimo. Pero la realidad es que usa preprocesadores y postprocesadores de CSS, .JSX en lugar de HTML, lenguajes que compilan a javascript, sourcemaps que permiten debugging, testing, minimizadores, empaquetado para distribución, ...

Webpack automatiza todas estas actividades. Merece la pena de aprenderlo.

Es un "code bundler". Sigue todo el árbol de dependencias para generar un paquete. Por defecto sólo procesa javascript, por lo que para cualquier otra cosa se requieren "loaders".

Supondremos que hemos inicializado el [proyecto](proyecto.md).

## Webpack

Instalamos [Webpack](https://webpack.js.org/) si no lo hemos hecho:
```
$ npm i webpack webpack-cli --save-dev
```

## Configuración
Hacemos una configuración mínima en **webpack.config.js**:

```js
var webpack = require('webpack');
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

en donde:
- Por una lado se requieren las librerías *webpack* y *path*
- La librería *path* sirve para componer las rutas de *BUILD_DIR* y *APP_DIR*. Vemos que la aplicación cliente se guarda en *src/client/app*.
- En la variable *config* e identifica el punto de entrada *entry* y la salida *output* (se indica la ruta y el nombre del fichero *bundle.js*).

!!! note ""
    Aquí se asume como punto de entrada un fichero con extensión .jsx. Esto requiere el uso de React.


## Babel
### Instalación
Instalaremos:

- babel-loader: es un plugin de webpack para Babel. Babel es un transpiler que convierte código ES6 en ES5. Esto posibilita que código escrito en ES6 sea soportado por navegadores que no lo soportan.
- babel-preset-es2015: preset para todos los plugins es2015.
- babel-preset-react: preset de una serie de plugins (entre ellos para transformar .jsx).

Se instala el loader y los presets:
```
$ npm i babel-loader babel-preset-es2015 babel-preset-react --save-dev
```

### Configuración
Configuramos **.babelrc**:

```js
{
  "presets" : ["es2015", "react"]
}
```

y añadimos en webpack la configuración del loader:

??? note "webpack.config.js"
    ```js hl_lines="13 14 15 16 17 18 19 20 21"
    var webpack = require('webpack');
    var path = require('path');

    var BUILD_DIR = path.resolve(__dirname, 'src/client/public');
    var APP_DIR = path.resolve(__dirname, 'src/client/app');

    var config = {
      entry: APP_DIR + '/index.jsx',
      output: {
        path: BUILD_DIR,
        filename: 'bundle.js'
      },
      module : {
          loaders : [
            {
              test : /\.jsx?/,
              include : APP_DIR,
              loader : 'babel'
            }
          ]
        }
    };

    module.exports = config;
    ```

## Ejecutar
Para construir el proyecto, ejecutamos:
```
$ node_modules/.bin/webpack -p
```



# Enlaces interesantes
[webpack patterns](https://github.com/larkintuckerllc/webpack-patterns)


# Otros

Normalmente creamos manualmente el fichero en el que incrustamos la aplicación:

??? note "src/client/app/index.html"
    ```html
    <!DOCTYPE html>
    <html lang="en">
    <head>
      <meta charset="UTF-8">
      <title>My App</title>
      <link rel="stylesheet" href="styles.css">
    </head>
    <body>
      <div id="app" />
    </body>
    </html>
    ```
