# Detalles: `src/main.js`
## Código
??? "./src/main.js"
    ```js
    import Vue from 'vue'
    import './plugins/vuetify'
    import App from './App.vue'
    import router from './router'
    import store from './store'
    import firebase from 'firebase'


    firebase.initializeApp({
     apiKey: "AIzaSyBZXlv0CUZBdPWvyzzi2eaBlXT764hoIDM",
     authDomain: "prueba-28ec5.firebaseapp.com",
     databaseURL: "https://prueba-28ec5.firebaseio.com",
     projectId: "prueba-28ec5"
    })

    /* eslint-disable */
    Vue.config.productionTip = false

    const unsubscribe = firebase.auth()
     .onAuthStateChanged((firebaseUser) => {
       new Vue({
         //el: '#app',
         router,
         store,
         render: h => h(App),
         created () {
           if (firebaseUser) {
             store.dispatch('autoSignIn', firebaseUser)
           }
         }
       }).$mount('#app')
       unsubscribe()
    })
    ```

## Importaciones
Importamos:

```js
import Vue from 'vue'
import './plugins/vuetify'
import App from './App.vue'
import router from './router'
import store from './store'
import firebase from 'firebase'
```

Estamos importando la librería `vue`, y `vuetify` como un plugin.

Importamos también `App.vue` como `App` (así se importan los componentes en Vue).

También importamos el `router` (contrala la vista a mostrar para cada path).

Importamos el `store` que es donde guardamos el estado de la aplicación (basado en Vuex).

Por último importamos Firebase. Firebase es un BaaS (Backend as a Service). Es una base de datos en tiempo real de Google.

## Inicializamos Firebase
Esto se hace con:

```js
firebase.initializeApp({
  apiKey: "AIzaSyBZXlv0CUZBdPWvyzzi2eaBlXT764hoIDM",
  authDomain: "prueba-28ec5.firebaseapp.com",
  databaseURL: "https://prueba-28ec5.firebaseio.com",
  projectId: "prueba-28ec5"
})
```

Esta información se consigue:

1. Vamos a la [Consola de Firebase](https://console.firebase.google.com/).
2. Elegimos el proyecto
3. Click en Autentificación
4. Click en Configuración Web.


## Production Tip
Lo siguiente:

```js
Vue.config.productionTip = false
```

¿qué hace?

## Hacemos

El siguiente código:

```js
const unsubscribe = firebase.auth()
  .onAuthStateChanged((firebaseUser) => {
    new Vue({
      //el: '#app',
      router,
      store,
      render: h => h(App),
      created () {
        if (firebaseUser) {
          store.dispatch('autoSignIn', firebaseUser)
        }
      }
    }).$mount('#app')
    unsubscribe()
})
```

es complicado de explicar.

Por un lado,
