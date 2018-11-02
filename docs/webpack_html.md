# Automatizar la generación del HTML
## Introducción
En el ejemplo [anterior](webpack_minimizer), vimos que generamos el fichero *index.html* manualmente.

Podemos ahorrarnos este paso mediante [html-webpack-plugin](https://webpack.js.org/plugins/html-webpack-plugin/) y *html-webpack-template*.

Lo instalamos:
```
$ npm i html-webpack-plugin html-webpack-template --save-dev
```

## Proyecto

```js tab="webpack.config.js" hl_lines="3 17 18 19 20"
const UglifyJSPlugin = require('uglifyjs-webpack-plugin');
var path = require('path');
var HtmlWebpackPlugin = require('html-webpack-plugin');

var BUILD_DIR = path.resolve(__dirname, 'src/client/build');
var APP_DIR = path.resolve(__dirname, 'src/client/app');

module.exports = {
  entry: APP_DIR + '/index.js',

  output: {
    path: BUILD_DIR,
    filename: 'bundle.js'
  },

  plugins: [
    new HtmlWebpackPlugin( {
      title: 'Webpack - ejemplo minimo',
      filename: 'index.html'
    }),

    new UglifyJSPlugin()
    ]
};
```

```js tab="./src/client/app/index.js"
window.console.log('Hola mundo');
```

y ejecutamos:
```bash
$ node_modules/.bin/webpack -p
$ firefox src/app/public/index.html
```
y pulsamos Ctrl+Shift+K

Se genera automáticamente el fichero:

??? note "src/client/build/index.html"
    ```html
    <!DOCTYPE html>
    <html>
      <head>
        <meta charset="UTF-8">
        <title>Webpack - ejemplo minimo</title>
      </head>
      <body>
      <script type="text/javascript" src="bundle.js"></script></body>
    </html>
    ```

# Template
El fichero HTML se puede modificar usando otro template.

Para ello existe: [html-webpack-template](https://github.com/jaketrent/html-webpack-template).

Pero no he conseguido que funcione appMountId.
```
    //template: 'node_modules/html-webpack-template/index.ejs',// require('html-webpack-template'),
```
