# Agenda
## Introducción
[PWA Vue Webpack Material Design Lite - Parte 1](https://blog.sicara.com/a-progressive-web-application-with-vue-js-webpack-material-design-part-1-c243e2e6e402<)

## Prerrequisitos
Instalamos:

```bash
$ yaourt -S vue-cli
$ yaourt -S ngrok
```

y creamos la aplicación:

```
$ vue init pwa pwa_agenda

? Project name pwa_agenda
? Project short name: fewer than 12 characters to not be truncated on homescreens (default: same as name)
? Project description Primer tutorial
? Author mantielero <josemaria.alkala@gmail.com>
? Vue build standalone
? Install vue-router? Yes
? Use ESLint to lint your code? Yes
? Pick an ESLint preset Standard
? Setup unit tests with Karma + Mocha? Yes
? Setup e2e tests with Nightwatch? Yes

   vue-cli · Generated "pwa_agenda".

   To get started:

     cd pwa_agenda
     npm install
     npm run dev

   Documentation can be found at https://vuejs-templates.github.io/webpack
```

lo que crea la siguiente estructura:

This process creates a project folder with following subfolders:

- build: contains webpack and vue-loader configuration files
- config: contains our app config (environments, parameters…)
- src: source code of our application
- static: images, css and other public assets
- test: unit test files propelled by Karma & Mocha

> El fichero **static/manifest.json** es el que permite que instalemos la aplicación.

y finalmente:

```
cd pwa_agenda
npm install
npm run dev
ngrok http 8080
```

Así podremos testear tanto en el navegador como directamente en un móvil o tablet.

## Creamos el esqueleto de las vistas
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
