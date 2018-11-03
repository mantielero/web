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


## Ejecutar
Para construir el proyecto, ejecutamos:
```
$ node_modules/.bin/webpack -p
```

Si tenemos instalado *webpack-cli* podemos llamarlo más cómodamente con *npm*:
```js package.json
"scripts": {
  "build": "webpack --mode production"
}
```

mediante:
```bash
$ npm run build
```

## Servidor de desarrollo
Para evitar estar ejecutando `npm run build` constantemente, interesa instalar el servidor de desarrollo:
```bash
npm i webpack-dev-server --save-dev
```

Y configuramos en:
```js package.json hl_lines="3"
"scripts": {
  "build": "webpack --mode production",
  "start": "webpack-dev-server --open --mode development"
}
```

De forma que ejecutaremos:
```bash
npm run start
```

y siempre que editemos un fichero del proyecto se actualizará el resultado del proyecto automáticamente.


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
