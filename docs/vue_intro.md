# Vue
## Introducción
Se trata de un framework para desarrollo frontend que parece ser sencillo.

## Instalamos Vue
En Archlinux:
```
$ yaourt -S vue-cli
$ yarn global add @vue/cli-init
```
y en general:
```
$ npm install -g vue-cli
```

## Inicializar un proyecto
Hacemos:
```
$ vue init webpack my-project
$ cd my-project
$ npm install
$ npm run dev
```

[Aquí](https://github.com/vuejs-templates/webpack) se peude ver todo lo que lleva incluido este template.



# Otros
```html tab="index.html"
<script src="https://unpkg.com/vue"></script>

<div id="app">
  <p>{{ message }}</p>
</div>
```

```js tab="index.js"
new Vue({
  el: '#app',
  data: {
    message: 'Hello Vue.js!'
  }
})
```
