# Agenda
## Introducción
[PWA Vue Webpack Material Design Lite - Parte 1](https://blog.sicara.com/a-progressive-web-application-with-vue-js-webpack-material-design-part-1-c243e2e6e402<)

[SPA - vue - firebase - vuex - vuetify](https://medium.com/@oleg.agapov/basic-single-page-application-using-vue-js-and-firebase-part-1-9e4c0c11a228)

El objetivo va a ser crear una aplicación que sea una agenda que:

- [ ] Sign-up
- [ ] Sign-in
- [ ] Routing
- [ ] Guardar información firebase

## Prerrequisitos
Instalamos:

```bash
$ yaourt -S vue-cli
$ yaourt -S ngrok
```
> Otra forma es `npm i -g @vue/cli`, que instala vue 3.


## Creamos el proyecto
Hacemos:

```bash
vue create agenda
```
y elegimos el default (babel, eslint).

```bash
cd agenda
vue add router
vue add vuex
vue add vuetify
npm run serve
```

en donde:

- router: instala *vue-router* que controla las vistas a mostrar en cada caso.
- vuex: es un store centralizado para el estado de la aplicación.
- vuetify: es el toolkit de visualización (basado en material design).

### Opcional - iconos
Podemos instalar los iconos localmente. Podemos elegir entre diferentes sets de iconos:

```bash
npm i -S vue-material-design-icons
```

## PWA
Inslamos *pwa*:

```bash
vue add pwa
```

### Estructura
La estructura creada:

- `./src/`: código fuente de la aplicación.

  - `./src/main.js`: es el punto de entrada a nuestra aplicación.
  - `./src/App.vue`: is a single file component attached to your application.
  - `./src/assets/`: para static assets
  - `./src/components`: contains other single file components
  - `./src/router/`: is for routing.

El resto de subdirectorios creados:

- build: contains webpack and vue-loader configuration files
- config: contains our app config (environments, parameters…)
- static: images, css and other public assets
- test: unit test files propelled by Karma & Mocha


## PWA
> El fichero **static/manifest.json** es el que permite que instalemos la aplicación.

y finalmente:

```
ngrok http 8080
```

Así podremos testear tanto en el navegador como directamente en un móvil o tablet.



### Firebase
- Firebase — for authentication, real-time database and hosting



# UI
## Página por defecto

### Página principal

Con vuetify, tenemos `<template>` y dentro `v-app`.

### Toolbar
En el ejemplo viene:

```vue
<v-toolbar app>
  <v-toolbar-title class="headline text-uppercase">
    <span>Vuetify</span>
    <span class="font-weight-light">MATERIAL DESIGN</span>
  </v-toolbar-title>
  <v-spacer></v-spacer>
  <v-btn
    flat
    href="https://github.com/vuetifyjs/vuetify/releases/latest"
    target="_blank"
  >
    <span class="mr-2">Latest Release</span>
  </v-btn>
</v-toolbar>
```

Información sobre [toolbars](https://vuetifyjs.com/en/components/toolbars). Vemos que estamos incluyendo:

- v-toolbar-title: título.
- v-spacer: hacemos espacio entre ambos componentes.
- v-btn: botón.

El botón se ha hecho con:

```vue
<v-btn
  flat
  href="https://github.com/vuetifyjs/vuetify/releases/latest"
  target="_blank"
>
  <span class="mr-2">Latest Release</span>
</v-btn>
```
### Contenido
En el contenido se referencia otro componente:

```vue
<v-content>
  <HelloWorld/>
</v-content>
```

Ese componente se carga posteriormente en el `<script>`:

```js
import HelloWorld from './components/HelloWorld'
```


# Nuevo UI
## Crear vistas
Lo primero es crear un pequeño "borrador" de las vistas que vamos a necesitar.

La idea es tener una página principal que tiene cierto layaut:

- toolbar
- contenido
- sidebar

En el sidebar daremos la opción de sign-up y sign-in en firebase:

- sign-up: no requiere autentificación
- sign-in: no requiere autentificación
- hola: requiere autentificación

[ejemplo](https://medium.com/@anas.mammeri/vue-2-firebase-how-to-build-a-vue-app-with-firebase-authentication-system-in-15-minutes-fdce6f289c3c)


## Página principal
El layout consistirá en:

- v-toolbar: será la barra superior.
- v-navigation-drawer: es un *sidebar*. Se ocultará o mostrará pulsando un botón.
- v-content: aquí meteremos las diferentes vistas.

La sección `<script>` exporta los valores por defecto (la inicialización). Básicamente pone el título de la toolbar y pone la sidebar en falso.

> Vemos que al hacer click en `<v-toolbar-side-icon>` el valor de sidebar le asigna el opuesto de lo que tuviera asignado.

El código será:

```vue tab="./src/App.vue"
<template>
  <v-app>
    <v-navigation-drawer v-model="sidebar" app>
    </v-navigation-drawer>

    <v-toolbar app>
      <span class="hidden-sm-and-up">
        <v-toolbar-side-icon @click="sidebar = !sidebar">
        </v-toolbar-side-icon>
      </span>
      <v-toolbar-title>{{ appTitle }}</v-toolbar-title>
      <v-spacer></v-spacer>
    </v-toolbar>

    <v-content>
    </v-content>

  </v-app>
</template>

<script>
  export default {
    data () {
      return {
        appTitle: 'Awesome App',
        sidebar: false
      }
    }
  }
</script>
```

Dentro de **src/components/**:

```vue tab="HomeView.vue"
<template>
  <ul class="list">
  </ul>
</template>
<script>
export default {
}
</script>
<style scoped>
  .list {
    width: 100%;
    padding: 0;
  }
</style>
```

```vue tab="DetailView.vue"
<template>
  <div class="card-image">
  </div>
</template>
<script>
  export default {
  }
</script>
<style scoped>
</style>
```

```vue tab="PostView.vue"
<template>
  <div class="waiting">
    Not yet available
  </div>
</template>
<script>
export default {
}
</script>
<style scoped>
  .waiting {
    padding: 10px;
    color: #555;
  }
</style>
```



> Hay que borrar el fichero **Hello.vue** que ya no es necesario.

## View: Sign up
En *src/components*:


## Rutado de las vistas

```js tab="src/router/index.js"
import Vue from 'vue'
import Router from 'vue-router'
import HomeView from '@/components/HomeView'
import DetailView from '@/components/DetailView'
import PostView from '@/components/PostView'

Vue.use(Router)

export default new Router({
  routes: [
    {
      path: '/',
      name: 'home',
      component: HomeView
    },
    {
      path: '/detail/:id',
      name: 'detail',
      component: DetailView
    },
    {
      path: '/post',
      name: 'post',
      component: PostView
    }
  ]
})
```

## Material Design Lite
### Instalación

```
npm install material-design-lite --save
```

```vue tag="src/App.vue"
<script>
  require('material-design-lite')
  ...
</script>
<style>
  @import url('https://fonts.googleapis.com/icon?family=Material+Icons');
  @import url('https://code.getmdl.io/1.2.1/material.blue-red.min.css');
</style>
```

Y se actualiza a:
```vue tab="src/App.vue"
<template>
  <div class="mdl-layout mdl-js-layout mdl-layout--fixed-header">
    <header class="mdl-layout__header">
      <div class="mdl-layout__header-row">
        <span class="mdl-layout-title">CropChat</span>
      </div>
    </header>
    <div class="mdl-layout__drawer">
      <span class="mdl-layout-title">CropChat</span>
      <nav class="mdl-navigation">
        <router-link class="mdl-navigation__link" to="/" @click.native="hideMenu">Home</router-link>
        <router-link class="mdl-navigation__link" to="/post" @click.native="hideMenu">Post a picture</router-link>
      </nav>
    </div>
    <main class="mdl-layout__content">
      <div class="page-content">
        <router-view></router-view>
      </div>
    </main>
  </div>
</template>
```
