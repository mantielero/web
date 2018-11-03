# React
## Introducción
[Reactjs](https://reactjs.org/) se autodefine como una librería para escribir interfaces de usuario. Es de Facebook. Se fundamenta en los conceptos que hay detrás de Elm:

- Modelo: son las diferentes variables que definen el estado.
- Update: son los diferentes métodos que pueden variar el estado. Se genera un nuevo estado.
- View: es consecuencia del estado.

Dado un modelo la vista (view) es única. Mediante la vista podemos llamar a los métodos de update, lo que nos lleva a un nuevo estado. Una ventaja de esto es que podemos volver a estados pasados (es como tener un Undo gratis).

La librería React se instala mediante:
```
$ npm i react react-dom -S
```

Con React se escriben componentes. Estos componentes se escriben en el lenguaje [JSX](https://reactjs.org/docs/introducing-jsx.html).

Para convertir el código .jsx en .js usaremos *babel* y *babel-preset-react*:
```
$ npm i @babel/core babel-loader @babel/preset-env @babel/preset-react --save-dev
```

Un [buen tutorial](https://www.valentinog.com/blog/react-webpack-babel/).

## Comprobamos la instalación

En lugar del fichero, *index.js* escribimos el fichero **src/client/app/index.jsx**.

También modificamos la configuración de webpack para que:

1. Compile los ficheros .jsx y los convierta en .js (también de ES6 a ES5).
2. Usamos [html-webpack-template](https://www.npmjs.com/package/html-webpack-template) para que genere `<div id=app />`

```js tab="webpack.config.js"
const UglifyJSPlugin = require('uglifyjs-webpack-plugin');
var path = require('path');
var HtmlWebpackPlugin = require('html-webpack-plugin');

var BUILD_DIR = path.resolve(__dirname, 'src/client/build');
var APP_DIR = path.resolve(__dirname, 'src/client/app');

module.exports = {
  entry: APP_DIR + '/index.jsx',

  output: {
    path: BUILD_DIR,
    filename: 'bundle.js'
  },

  module: {
    rules: [
      {
        test : /\.jsx?/,
        include : APP_DIR,
        use: {
          loader : 'babel-loader'
        }
      },
      {
        test: /\.js$/,
        exclude: /node_modules/,
        use: {
          loader: "babel-loader"
        }
      }
    ]
  },

  plugins: [
    new HtmlWebpackPlugin( {
      title: 'Webpack - ejemplo minimo',
      filename: 'index.html',
      // Required
      inject: false,
      template: require('html-webpack-template'),
      // Optional
      appMountId: 'app'      

    }),

    new UglifyJSPlugin()
    ]
};
```

```jsx tab="src/client/app/index.jsx"
import React from 'react';
import {render} from 'react-dom';

class App extends React.Component {
  render () {
    return <p>Hello React!</p>;
  }
}

render(<App/>, document.getElementById('app'));
```

En la configuración de webpack tenemos una serie de reglas (module -> rules) que usan el loader **babel-loader**:

1. Convertir los ficheros .jsx en .js
2. Convierte los ficheros .js en formato ES6 a formato ES5.

A partir del fichero **index.jsx** webpack generará el fichero **bundle.js**.



## Desarrollando en React
### Introducción
Una cosa que estaba muy bien de Elm es el hecho de no modificar el estado (teníamos que crear un estado nuevo).
### Principio: Container/Presentational
En donde:

- Container: es el componente que contiene el estado y las funciones que controlan el estado.
- Presentational: es la parte de visualización.

> Curioso porque parece que uno de los principios de .jsx es precisamente que la lógica y la visualización van muy vinculadas y por ello se pone todo junto.




## React-Toolbox

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

# Links

[Building a React app with webpack](https://egghead.io/lessons/react-building-a-react-js-app-up-and-running-with-react-and-webpack)

# Pregenerados
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
