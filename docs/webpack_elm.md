# Webpack with Elm

## Instalación para Elm

Para trabajar con Elm, requerimos los siguientes paquetes::
```
$ npm i webpack webpack-dev-middleware webpack-dev-server elm-webpack-loader file-loader style-loader css-loader url-loader --save-dev
```

en donde:

- webpack: es el paquete básico
- webpack-dev-server: es un servidor que nos permite testear nuestra API.
- webpack-dev-middleware: ??
- elm-webpack-loader: el específico para ficheros Elm.
- file-loader: ??
- style-loader y css-loader: para ficheros CSS.
- url-loader

Por último, indicamos a NPM que son paquetes de nuestro entorno de desarrollo (no dependencias de nuestro código).

## Funcionamiento básico

Un fichero de configuración básico **webpack.config.js** tiene la siguiente pinta:

```js
  var path = require("path");

  module.exports = {
    entry: {
      app: [
        './src/index.js'
      ]
    },

    output: {
      path: path.resolve(__dirname + '/dist'),
      filename: '[name].js',
    },

    module: {
      loaders: [
        {
          test: /\.(css|scss)$/,
          loaders: [
            'style-loader',
            'css-loader',
          ]
        },
        {
          test:    /\.html$/,
          exclude: /node_modules/,
          loader:  'file?name=[name].[ext]',
        },
        {
          test:    /\.elm$/,
          exclude: [/elm-stuff/, /node_modules/],
          loader:  'elm-webpack',
        },
        {
          test: /\.woff(2)?(\?v=[0-9]\.[0-9]\.[0-9])?$/,
          loader: 'url-loader?limit=10000&minetype=application/font-woff',
        },
        {
          test: /\.(ttf|eot|svg)(\?v=[0-9]\.[0-9]\.[0-9])?$/,
          loader: 'file-loader',
        },
      ],

      noParse: /\.elm$/,
    },

    devServer: {
      inline: true,
      stats: { colors: true },
    },


  };
```

En donde **./src/index.js** es el punto de entrada a nuestra aplicación. Los ficheros generados, los guardará en **./dist/**. Por otro lado, define una serie de filtros en función del tipo de archivo: css, html, ... para que use un loader u otro. Por último, también configura el servidor de desarrollo.

Ahora requerimos un **fichero.html** sobre el que cargaremos el fichero javascript generado a partir de nuestro Elm. Y diremos el componente sobre el que lo montamos. Algo de la pinta:

```html

  <!DOCTYPE html>
  <html>
    <head>
      <meta charset="utf-8" />
      <title>Elm SPA example</title>
    </head>
    <body>
      <div id="main">Loading...</div>
      <script src="/app.js"></script>
    </body>
  </html>
```

index.js
--------

Este es el punto de entrada que buscará webpack (lo definimos en el fichero de configuración) para crear en bundle.

Un ejemplo es el siguiente:

```js
  'use strict';

  require('basscss/css/basscss.css');
  require('font-awesome/css/font-awesome.css');

  // Require index.html so it gets copied to dist
  require('./index.html');

  var Elm = require('./Main.elm');
  var mountNode = document.getElementById('main');

  // The third value on embed are the initial values for incomming ports into Elm
  var app = Elm.embed(Elm.Main, mountNode);
```

En el que se está requiriendo CSS, el HTML raíz que representa nuestra aplicación y el fichero Elm que se montará en componente del .html con identificador "main" en este ejemplo.

package.json
------------

Podemos modificar la sección **script** para que aparezcan las tareas típicas que disponemos con webpack:

```js
"scripts": {
    "api": "node api.js",
    "build": "webpack",
    "watch": "webpack --watch",
    "dev": "webpack-dev-server --port 3000"
},
```

Vemos que tenemos un **node api.js**. No es un error, es una llamada a nuestro test-harness del servidor.

De esta forma podremos:

- Ejecutar nuestro servidor de mentira (supongo que usando json-server)::
```
  $ npm run api
```
- Recopilar todo el código y dejarlo en **dist**::
```
  $ npm run build
```
- Mirar únicamente aquello que cambie y dejarlo en **dist**::
```
  $ npm run watch
```
- Ejecutar el servidor de desarrollo de webpack::
```
  $ npm run dev
```

Uso
---

Normalmente ejecutaremos nuestro servidor de mentira en una consola::
```
  $ npm run api
```

y nuestra aplicación mediante el servidor de desarrollo de webpack::
```
  $ npm run dev
```
Podremos interactuar con nuestra aplicación en http://localhost:3000 de acuerdo con la configuración realizada.
