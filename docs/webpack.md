# Bundling

## Introducción
El código web es complejo. Usa HTML, CSS3 y javascript como mínimo. Pero la realidad es que usa preprocesadores y postprocesadores de CSS, .JSX en lugar de HTML, lenguajes que compilan a javascript, sourcemaps que permiten debugging, testing, minimizadores, empaquetado para distribución, ...

Webpack automatiza todas estas actividades. Merece la pena de aprenderlo.

Es un "code bundler". Sigue todo el árbol de dependencias para generar un paquete. Por defecto sólo procesa javascript, por lo que para cualquier otra cosa se requieren "loaders".

## Partimos de un proyecto

Iniciamos el proyecto::
```
$ mkdir proyecto
$ cd proyecto
$ npm init -y
```

y creamos la siguiente estructura:
```
$ mkdir -p src/components
$ mkdir dist
```

## Webpack

Instalamos [Webpack](https://webpack.js.org/):
```
$ npm i webpack --save-dev
```

y hacemos una configuración mínima en **webpack.config.js**:

```js
var webpack = require('webpack');
var path = require('path');

var BUILD_DIR = path.resolve(__dirname, 'src/client/public');
var APP_DIR = path.resolve(__dirname, 'src/client/app');

var config = {
  entry: APP_DIR + '/index.jsx',
  output: {
    path: BUILD_DIR,
    filename: 'bundle.js'
  }
};

module.exports = config;
```

Instalamos lo necesario para soportar ES6:
```
$ npm i babel-loader babel-preset-es2015 babel-preset-react --save-dev
```

configuramos **.babelrc**:

```js
{
  "presets" : ["es2015", "react"]
}
```

y añadimos en webpack la configuración del loader:

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

También queremos generar automáticamente el fichero HTML de entrada. Para ello haremos::
```
$ npm i html-webpack-plugin html-webpack-template --save-dev
```
Creamos un fichero **src/client/app/index.html** que usaremos en webpack como template:

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

y modificamos el fichero de configuración:

```js
var webpack = require('webpack');
var path = require('path');
var HtmlWebpackPlugin = require('html-webpack-plugin');

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
    },
  plugins: [new HtmlWebpackPlugin( {
    template: 'node_modules/html-webpack-template/index.ejs',// require('html-webpack-template'),
    title: 'My App',
    filename: 'index.html'
    appMountId: 'app',
    })]
};

module.exports = config;
```

En teoría con html-webpack-plugin y html-webpack-template debería ir bien. Pero no he conseguido que funcione appMountId.



## React

Será lo que usemos para la interfaz gráfica::
```
$ npm i react react-dom -S
```

### Comprobamos la instalación

Creamos el fichero **src/client/app/index.jsx**:

```js
import React from 'react';
import {render} from 'react-dom';

class App extends React.Component {
  render () {
    return <p> Hello React!</p>;
  }
}

render(<App/>, document.getElementById('app'));
```

Comprobamos que se genera el fichero **bundle.js** correctamente.


### React-Toolbox

Será el que usemos::
```
$ npm i react-toolbox -S
```

y requiere los siguientes loaders de webpack::
```
$ npm i sass-loader css-loader style-loader --save-dev
```

y además::
```
$ npm i node-sass --save-dev
$ npm i react-addons-css-transition-group --save
```

Cambiamos el fichero **src/cliente/app/index.jsx**:

```js
```

## Links

[Building a React app with webpack](https://egghead.io/lessons/react-building-a-react-js-app-up-and-running-with-react-and-webpack)

## Pregenerados
Lo más rápido y sencillo es clonar por ejemplo el siguiente repositorio:
```
$ git clone https://github.com/react-toolbox/react-toolbox-example
```

También podemos clonar el repositorio:
```
$ git clone https://github.com/callemall/material-ui.git
```

y copiar el directorio::
```
$ cp -R material-ui/material-ui/examples/webpack-example ./
```

La primera es una aplicación webpack/react-toolbox que funciona correctamente y que está correctamente configurada. Para hacerla funcionar, basta con:
```
$ npm install
$ npm start
```

Veremos la aplicación funcionando en http://localhost:8080

La segunda es material-ui. Funciona igual, pero funciona en: http://localhost:3000


> Como interfaz gráfica hay varios: material-ui, http://www.materialup.com/, react-mdl, react-toolbox.
