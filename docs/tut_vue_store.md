# Store (Vuex)
## Código

??? "./src/store.js"
    ```js
    import Vue from 'vue'
    import Vuex from 'vuex'
    import firebase from 'firebase'
    import router from '@/router'

    Vue.use(Vuex)
    // const store =
    export default new Vuex.Store({
      state: {
        appTitle: 'My Awesome App',
        user: null,
        error: null,
        loading: false
      },

      mutations: {
        setUser (state, payload) {
          state.user = payload
        },
        setError (state, payload) {
          state.error = payload
        },
        setLoading (state, payload) {
          state.loading = payload
        }
      },

      actions: {

        userSignUp ({commit}, payload) {
          commit('setLoading', true)
          firebase.auth().createUserWithEmailAndPassword(payload.email, payload.password)
            .then(firebaseUser => {
              commit('setUser', {email: firebaseUser.user.email})
              commit('setLoading', false)
              router.push('/home')
            })
            .catch(error => {
              commit('setError', error.message)
              commit('setLoading', false)
            })
        },

        userSignIn ({commit}, payload) {
          commit('setLoading', true)
          firebase.auth().signInWithEmailAndPassword(payload.email, payload.password)
            .then(firebaseUser => {
              commit('setUser', {email: firebaseUser.user.email})
              commit('setLoading', false)
              commit('setError', null)
              router.push('/home')
            })
            .catch(error => {
              commit('setError', error.message)
              commit('setLoading', false)
            })
        },

        autoSignIn ({commit}, payload) {
          commit('setUser', {email: payload.email})
        },

        userSignOut ({commit}) {
          firebase.auth().signOut()
          commit('setUser', null)
          router.push('/')
        }
      },

      getters: {
        isAuthenticated (state) {
          return state.user !== null && state.user !== undefined
        }
      }
    })
    ```

## Importaciones
Importamos lo siguiente:

```js
import Vue from 'vue'
import Vuex from 'vuex'
import firebase from 'firebase'
import router from '@/router'
```

Importamos `vue`, `vuex`, `firebase` (para poder hacer llamadas al signin/signup), `router` (para poder redireccionar a otras páginas).

## Iniciamos con el store
Lo hacemos mediante:

```js
Vue.use(Vuex)
```

## Creamos el store
### Estado
Tenemos las siguientes cuatro variables que controlan el estado de la aplicación:

```js
state: {
  appTitle: 'My Awesome App',
  user: null,
  error: null,
  loading: false
},
```

Básicamente, podemos controlar el título de la aplicación, el usuario que está logueado, el mensaje de error en caso de que se produzca, y `loading` que sirve para deshabilitar botones mientras esperamos.

### Mutaciones
Las mutaciones se encargan de cambiar el estado de forma controlada:

```js
mutations: {
  setUser (state, payload) {
    state.user = payload
  },
  setError (state, payload) {
    state.error = payload
  },
  setLoading (state, payload) {
    state.loading = payload
  }
},
```

Vemos que podemos:

- asignar un usuario
- asignar un error
- asignar el loading

> Son los setters

### Acciones
#### Introducción
Las [acciones](https://vuex.vuejs.org/guide/actions.html) son similares a las mutaciones. La diferencia reside en:

- las acciones hacen `commit` de mutaciones
- las acciones pueden contener operaciones asíncronas


#### userSignUp

```js
userSignUp ({commit}, payload) {
  commit('setLoading', true)
  firebase.auth().createUserWithEmailAndPassword(payload.email, payload.password)
    .then(firebaseUser => {
      commit('setUser', {email: firebaseUser.user.email})
      commit('setLoading', false)
      router.push('/home')
    })
    .catch(error => {
      commit('setError', error.message)
      commit('setLoading', false)
    })
},
```

### userSignIn
```js
userSignIn ({commit}, payload) {
  commit('setLoading', true)
  firebase.auth().signInWithEmailAndPassword(payload.email, payload.password)
    .then(firebaseUser => {
      commit('setUser', {email: firebaseUser.user.email})
      commit('setLoading', false)
      commit('setError', null)
      router.push('/home')
    })
    .catch(error => {
      commit('setError', error.message)
      commit('setLoading', false)
    })
},
```

### autoSignIn

```js
autoSignIn ({commit}, payload) {
  commit('setUser', {email: payload.email})
},
```

### userSignOut

```js
userSignOut ({commit}) {
  firebase.auth().signOut()
  commit('setUser', null)
  router.push('/')
}
```

### Getters

```js
getters: {
  isAuthenticated (state) {
    return state.user !== null && state.user !== undefined
  }
}
```
