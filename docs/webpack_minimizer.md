# Ejemplo mínimo
## Introducción
Basado en [webpack by example - part 1](https://medium.com/front-end-hacking/webpack-by-example-part-1-1d07bc42006a).

Similar al ejemplo [mínimo](webpack_minimo), pero ahora minimizamos el espacio ocupado para que carge más rápido. Para ello usaremos el plugin Uglify.

Lo instalamos:
```
$ npm i uglifyjs-webpack-plugin --save-dev
```

## Proyecto

```js tab="webpack.config.js" hl_lines="1 15"
const UglifyJSPlugin = require('uglifyjs-webpack-plugin');
var path = require('path');

var BUILD_DIR = path.resolve(__dirname, 'src/client/public');
var APP_DIR = path.resolve(__dirname, 'src/client/app');

var config = {
  entry: APP_DIR + '/index.js',

  output: {
    path: BUILD_DIR,
    filename: 'bundle.js'
  },

  plugins: [new UglifyJSPlugin()]
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

y ejecutamos:
```bash
$ node_modules/.bin/webpack -p
$ firefox src/app/public/index.html
```
y pulsamos Ctrl+Shift+K
