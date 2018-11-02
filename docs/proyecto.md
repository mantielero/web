# Proyecto
## Introducción
Suponemos que tenemos [nodejs](https://nodejs.org/es/) instalado en el sistema. Ello nos proporciona la herramienta de gestión de paquetes **npm**.

## Creamos el proyecto
Iniciamos el proyecto::
```bash
$ mkdir proyecto
$ cd proyecto
$ npm init -y
```

que genera el fichero:

??? note "package.json"
    ```js
    {
      "name": "pwa",
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

## Estructura
Creamos la siguiente estructura de directorios:
```
$ mkdir -p src/components
$ mkdir -p src/client/app/
$ mkdir dist
```
en donde:

- src: contendrá el código fuente
- src/client/app/: es donde guardaremos la aplicación
- src/components: los componentes
- dist: la distribución del programa.

## Dependencias
Podremos instalarlas bien globalmente o específicamente para el proyecto.

También podremos diferenciarlas entre requeridas por el ejecutable o requeridas únicamente para el desarrollo.
### Webpack
El proceso de bundling sólo tiene lugar durante el desarrollo, por lo que haremos:
```bash
$ npm i webpack --save-dev
```

Esto descarga webpack y un montón de dependencias (más de 300).

Además:

- node_modules/: es la carpeta en la que se almacenan los paquetes.
- package.json: ha cambiado

??? note "package.json"
    ```json hl_lines="12 13 14"
    {
      "name": "pwa",
      "version": "1.0.0",
      "description": "",
      "main": "index.js",
      "scripts": {
        "test": "echo \"Error: no test specified\" && exit 1"
      },
      "keywords": [],
      "author": "",
      "license": "ISC",
      "devDependencies": {
        "webpack": "^4.23.1"
      }
    }
    ```
- package-lock.json: contiene la lista de todos los paquetes.
