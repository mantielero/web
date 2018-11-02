# Desarrollo Web
## Introducción
Estás son mis notas personales en cuanto a desarrollo web.

Proceso de aprendizaje para un [Frontend web developer](https://medium.com/tech-tajawal/modern-frontend-developer-in-2018-4c2072fa2b9c).

## Introducción
### Imprescindibles
Los tres ingredientes básicos de la tecnología web son:

#### HTML
Es necesario estar familiarizado con [HTML5](https://developer.mozilla.org/en-US/docs/Web/HTML). Aunque el conocimiento necesario para hacer una SPA (Single Page Application) no es necesario que sea muy grande.
#### CSS3
[CSS3](https://developer.mozilla.org/en-US/docs/Web/CSS) sirve para controlar el estilo de visualización.

Preferentemente usaremos frameworks en los que diseñadores profesionales ya han hecho el trabajo duro.
#### JavaScript
El ES5 era horrible. Tiene muchas tantas cosas feas que han nacido muchos lenguajes que compilan a [Javascript](https://developer.mozilla.org/en-US/docs/Web/JavaScript).

El [ES6](https://codeburst.io/es6-tutorial-for-beginners-5f3c4e7960be) tiene buena pinta. Recomiendo darle una oportunidad a ES6.

Javascript puede ejecutarse en un navegador o con [nodejs](https://nodejs.org/es/).


## Otros lenguajes de programación

### Elm
Inspirado en Haskell, ha servido de inspiración a React.
### Otros
Typescript (de Microsoft), Dart (de Google), Transcrypt para Python, CoffeScript (extensiones de Atom), ... y un largo etc.

## Versatilidad
Con la tecnología podemos hacer webs, aplicaciones de escritorio (ver *electron*), aplicaciones para móvil (react native), aplicaciones web progresivas (que permiten que se instalen en el móvil).

> Es importante, pensar en *mobile first* cuando se hace el front end de una aplicación.

También se pueden crear aplicaciones para servidor.

## Complejidad
Esa versatilidad trae mucha complejidad:
- Lenguajes que compilan a javascript.
- El debugging se complica (sourcemaps)
- Librerías para testing
- La documentación
- Renderizado en el cliente o en el servidor
- El backend
- Estás en internet, por lo que necesitas autentificación, seguridad, ...
- Diversos frameworks
- Cada año una nueva moda.

## Entorno
### Dependencias
Para la descarga de paquetes y gestión de dependencias se usa típicamente:

- **npm**: para el lado servidor
- **bower**: para el lado cliente

### Empaquetado (bundling?)
Tenemos para empaquetado de software:

- webpack

### Aplicaciones de escritorio
Para el desarrollo de aplicaciones de escritorio, disponemos de:

- electron
- http://nwjs.io (node webkit)

### Backend
Para hacer servidores, disponemos de *express* o el más moderno **koajs**.

### Frontend - frameworks
Tenemos frameworks MVC como es el caso de **angular**. También tenemos sistemas como **react** para la parte visual.

### Móviles
Para hacer aplicaciones móviles, tenemos:

- phonegap/cordova: que básicamente ejecuta una página web dentro de una vista de página web.
- React Native: que ejecuta widgets nativos en el móvil.

Planteamos la aplicación puramente javascript, tanto para el servidor como para el cliente:

Existen toolkits que proporcionan componentes gráficos como jquerymobile, dojo, bootstrap, ionic, ...

### Librerías interesantes
Y miles de librerías muy interesantes como D3, openlayers, ...

### Automatización de tareas
Para la automatización de tareas tenemos *grunt* o mejor **gulp**.

También existen generadores como **yeoman**, que sirven para crear una estructura (scaffolding) razonable en función del proyecto que se quiere crear.

### Bases de datos
En el lado servidor, nos puede interesar tener una base de datos como RethinkDB o como MongoDB. GraphQL?

También tenemos tecnologías como coffeescript, typescript o elm.

Para hablar con Arduino, podemos hacer una aplicación móvil y que use MQTT (librería Paho como cliente y Mosquitto en un PC como servidor).

### IDE


Yo estoy usando **atom**, que es muy extensible. Por ejemplo, puede extenderse con **PlatformIO** para programar en Arduino y **Nuclide** para programar en React Native.


## Primeros pasos
Instalar **atom** y **nodejs**. Así tendremos instalado el gestor de paquetes **npm**.
