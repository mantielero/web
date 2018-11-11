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
Lo siguiente se explica [aquí](https://vuejs.org/v2/api/#productionTip):

```js
Vue.config.productionTip = false
```

> Set this to false to prevent the production tip on Vue startup.


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

No entiendo bien este código.

> This code is explained [here](https://stackoverflow.com/questions/37370224/firebase-stop-listening-onauthstatechanged), [here](https://forum.vuejs.org/t/firebase-auth-and-vue-router/3086/3) and [here](https://groups.google.com/forum/?hl=vi#!topic/firebase-talk/836OyVNd_Yg). Long story short, we call our Vue instance after observer onAuthStateChanged finish a check. And after resolving user’s state we stop the observer by calling unsubscribe().

### Instancia de Vue
Por un lado se crea la instancia de Vue y se monta en `#app` de `index.html`:

??? "Instancia de Vue montada en `#app`"
    ```js hl_lines="3 4 5 6 7 8 9 10 11 12 13"
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

### Observer
Según la [documentación](https://firebase.google.com/docs/reference/js/firebase.auth.Auth#onAuthStateChanged), la función `onAuthStateChanged` devuelve la función `unsubscribe` para el observador.

According to the documentation, the onAuthStateChanged() function returns

    The unsubscribe function for the observer.

So you can just:

```js
var unsubscribe = firebase.auth().onAuthStateChanged(function (user) {
    // handle it
});
```

And then:

```js
unsubscribe();
```
