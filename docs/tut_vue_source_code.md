## Código fuente

En el directorio `src` tenemos el código:

```js tab="main.js"
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

```js tab="store.js"
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

```js tab="router.js"
import Vue from 'vue'
import Router from 'vue-router'
import firebase from 'firebase'

const routerOptions = [
  { path: '/', component: 'Landing' },
  { path: '/signin', component: 'Signin' },
  { path: '/signup', component: 'Signup' },
  { path: '/home', component: 'Home', meta: { requiresAuth: true } },
  { path: '*', component: 'NotFound' }
]

const routes = routerOptions.map(route => {
  return {
    ...route,
    component: () => import(`@/components/${route.component}.vue`)
  }
})

Vue.use(Router)

const router = new Router({
  mode: 'history',
  routes
})

router.beforeEach((to, from, next) => {
  const requiresAuth = to.matched.some(record => record.meta.requiresAuth)
  const isAuthenticated = firebase.auth().currentUser
  if (requiresAuth && !isAuthenticated) {
    next('/signin')
  } else {
    next()
  }
})

export default router
```

Los ficheros son:

- `main.js`: es el punto de entrada a la aplicación. Llama a la aplicación `App.vue`, al store vuex, el router, inicializa firebase y vue.
- `router.js`: indica las vistas y las rutas adecuadas.
- `store.js`: contiene el store basado en Vuex. Pero también importa Firebase para poder hacer la autentificación y el router para poder ir a la vista deseada.

### Vistas
La vista principal es:

```vue tab="App.vue"
<template>
  <v-app>
    <v-navigation-drawer v-model="sidebar" app>
      <v-list>
        <v-list-tile
          v-for="item in menuItems"
          :key="item.title"
          :to="item.path">
          <v-list-tile-action>
            <v-icon>{{ item.icon }}</v-icon>
          </v-list-tile-action>
          <v-list-tile-content>{{ item.title }}</v-list-tile-content>
        </v-list-tile>
        <v-list-tile @click="userSignOut" v-if="isAuthenticated">
          <v-list-tile-action>
            <v-icon>exit_to_app</v-icon>
          </v-list-tile-action>
          <v-list-tile-content>Sign Out</v-list-tile-content>
        </v-list-tile>
      </v-list>
    </v-navigation-drawer>

    <v-toolbar app>
      <span class="hidden-sm-and-up">
        <v-toolbar-side-icon @click="sidebar = !sidebar">
        </v-toolbar-side-icon>
      </span>
      <v-toolbar-title>
        <router-link to="/" tag="span" style="cursor: pointer">
          {{ appTitle }}
        </router-link>
      </v-toolbar-title>
      <v-spacer></v-spacer>
      <v-toolbar-items class="hidden-xs-only">
        <v-btn
          flat
          v-for="item in menuItems"
          :key="item.title"
          :to="item.path">
          <v-icon left dark>{{ item.icon }}</v-icon>
          {{ item.title }}
        </v-btn>
        <v-btn flat @click="userSignOut" v-if="isAuthenticated">
          <v-icon left>exit_to_app</v-icon>
          Sign Out
        </v-btn>
      </v-toolbar-items>
    </v-toolbar>

    <v-content>
      <router-view></router-view>
    </v-content>

  </v-app>
</template>

<script>
  export default {
    data () {
      return {
        // appTitle: 'Awesome App',
        sidebar: false
        // menuItems: [
        //   { title: 'Home', path: '/home', icon: 'home' },
        //   { title: 'Sign Up', path: '/signup', icon: 'face' },
        //   { title: 'Sign In', path: '/signin', icon: 'lock_open' }
        // ]
      }
    },
    computed: {
      appTitle () {
        return this.$store.state.appTitle
      },
      isAuthenticated () {
        return this.$store.getters.isAuthenticated
      },
      menuItems () {
        if (this.isAuthenticated) {
          return [
            { title: 'Home', path: '/home', icon: 'home' }
          ]
        } else {
          return [
            { title: 'Sign Up', path: '/signup', icon: 'face' },
            { title: 'Sign In', path: '/signin', icon: 'lock_open' }
          ]
        }
      }
    },
    methods: {
      userSignOut () {
        this.$store.dispatch('userSignOut')
      }
    }
  }
</script>
```

Y en `./src/components/`:

```vue tab="Landing.vue"
<template>
  <v-container fluid>
    <v-layout column>
      <v-flex xs12 class="text-xs-center" mt-5>
        <h1>Landing page</h1>
      </v-flex>
    </v-layout>
  </v-container>
</template>

<script>
export default {}
</script>
```

```vue tab="Signin.vue"
<template>
  <v-container fluid>
    <v-layout row wrap>
      <v-flex xs12 class="text-xs-center" mt-5>
        <h1>Sign In</h1>
      </v-flex>
      <v-flex xs12 sm6 offset-sm3 mt-3>
        <form @submit.prevent="userSignIn">
          <v-layout column>
            <v-flex>
              <v-alert type="error" dismissible v-model="alert">
                {{ error }}
              </v-alert>
            </v-flex>
            <v-flex>
              <v-text-field
                name="email"
                label="Email"
                id="email"
                type="email"
                v-model="email"
                required></v-text-field>
            </v-flex>
            <v-flex>
              <v-text-field
                name="password"
                label="Password"
                id="password"
                type="password"
                v-model="password"
                required></v-text-field>
            </v-flex>
            <v-flex class="text-xs-center" mt-5>
              <v-btn color="primary" type="submit">Sign In</v-btn>
            </v-flex>
          </v-layout>
        </form>
      </v-flex>
    </v-layout>
  </v-container>
</template>

<script>
export default {
  data () {
    return {
      email: '',
      password: '',
      alert: false
    }
  },
  methods: {
    userSignIn () {
      this.$store.dispatch('userSignIn', { email: this.email, password: this.password })
    }
  },
  computed: {
    error () {
      return this.$store.state.error
    },
    loading () {
      return this.$store.state.loading
    }
  },
  watch: {
    error (value) {
      if (value) {
        this.alert = true
      }
    },
    alert (value) {
      if (!value) {
        this.$store.commit('setError', null)
      }
    }
  }
}
</script>
```

```vue tab="Signup.vue"
<template>
  <v-container fluid>
    <v-layout row wrap>
      <v-flex xs12 class="text-xs-center" mt-5>
        <h1>Sign Up</h1>
      </v-flex>
      <v-flex xs12 sm6 offset-sm3 mt-3>
        <form @submit.prevent="userSignUp">
          <v-layout column>

            <v-flex>
              <v-alert type="error" dismissible v-model="alert">
                {{ error }}
              </v-alert>
            </v-flex>

            <v-flex>
              <v-text-field
                name="email"
                label="Email"
                id="email"
                type="email"
                v-model="email"
                required></v-text-field>
            </v-flex>
            <v-flex>
              <v-text-field
                name="password"
                label="Password"
                id="password"
                type="password"
                v-model="password"
                required></v-text-field>
            </v-flex>
            <v-flex>
              <v-text-field
                name="confirmPassword"
                label="Confirm Password"
                id="confirmPassword"
                type="password"
                v-model="passwordConfirm"
                :rules="[comparePasswords]"
                required
                ></v-text-field>
            </v-flex>
            <v-flex class="text-xs-center" mt-5>
              <v-btn color="primary" type="submit" :disabled="loading">Sign Up</v-btn>
            </v-flex>
          </v-layout>
        </form>
      </v-flex>
    </v-layout>
  </v-container>
</template>

<script>
export default {
  data () {
    return {
      email: '',
      password: '',
      passwordConfirm: '',

      alert: false
    }
  },

  computed: {
    comparePasswords () {
        return this.password === this.passwordConfirm ? true : 'Passwords don\'t match'
      },
    error () {
      return this.$store.state.error
    },
    loading () {
      return this.$store.state.loading
    }
  },

  methods: {
    userSignUp () {
      if (this.comparePasswords !== true) {
        return
      }
      this.$store.dispatch('userSignUp', { email: this.email, password: this.password })
    }
  },

  watch: {
    error (value) {
      if (value) {
        this.alert = true
      }
    },
    alert (value) {
      if (!value) {
        this.$store.commit('setError', null)
      }
    }
  }

}
</script>
```

```vue tab="Home.vue"
<template>
  <v-container fluid>
    <v-layout row wrap>
      <v-flex xs12 class="text-xs-center" mt-5>
        <h1>Home page</h1>
      </v-flex>
      <v-flex xs12 class="text-xs-center" mt-3>
        <p>This is a user's home page</p>
      </v-flex>
    </v-layout>
  </v-container>
</template>

<script>
export default {}
</script>
```

```vue tab="NotFound.vue"
<template>
  <v-container fluid>
    <v-layout row wrap>
      <v-flex xs12 class="text-xs-center" mt-5>
        <h1>Error - page not found</h1>
      </v-flex>
    </v-layout>
  </v-container>
</template>

<script>
export default {}
</script>
```

En estas vistas, observamos que todos los componentes (o vistas) tienen dos partes:

- El `<template>`
- El `<script>`

Dentro de `<template>` hay un contenedor: `<v-container fluid>` (la propiedad *fluid* extiendo al ancho completo).

Dentro de `<v-container fluid>` tenemos el componente `<v-layout row wrap>`. Sirve para separar secciones. Contiene las propiedades:

- row: pone flex direction a row
- wrap: allows children to wrap within the container if the elements use more than 100%.

Dentro de `<v-layout>` tendremos `<v-flex>` (puede haber varias instancias de `v-flex`). Que ajusta el [grid](https://vuetifyjs.com/en/layout/grid). Entre las posibles propiedades tenemos:

- (size)(1-12): (ej. xs12) en donde el tamaño puede ser:

  - xs: extra small
  - sm: small
  - md: medium
  - lg: large
  - xl: extra large

- [Spacing](https://vuetifyjs.com/en/layout/spacing): por ejemplo, `mt-5` pone un margen superior de 5. En el enlace viene un playground para ver el ajuste adecuado.

Ajustar el [alineamiento](https://vuetifyjs.com/en/layout/alignment) del texto mediante `class="lo que sea"`:

```html
<p class="text-lg-right">Right align on large viewport sizes</p>
<p class="text-md-center">Center align on medium viewport sizes</p>
<p class="text-sm-left">Left align on small viewport sizes</p>
<p class="text-xs-center">Center align on all viewport sizes</p>
<p class="text-xs-right">Right align on all viewport sizes</p>
```

- lg: pantallas grandes
- md: pantallas medianas
- sm: pantallas pequeñas
- xs: todas las pantallas


Dentro de flex, tenemos componentes como:

- `<h1>`: heading
- `<p>`: párrafo
- `<form>`: dentro puede haber `v-layout` con los correspondientes `flex`.
- `<v-text-field>`:
- `<v-btn>`:
- `<v-alert>`:  


### Aplicación
La página principal de la aplicación es un poco especial.

Dentro de `<template>` tenemos `<v-app>`. Dentro tendremos componentes.


Digno de mención es la lista. La lista comienza con `<v-list>` y después contiene tiles `<v-list-tile>`. Dentro del tile podemos tener:

- `<v-list-tile-action>`: dentro tenemos `<v-icon>`
- `<v-list-tile-content>`

En los `<v-list-tile>` puede tener propiedades como:

- `@click=`: indica la función a la que llamará cuando se haga click en el elemento.
- `v-if`: muestra el tile si el valor de la variable asociada es verdadero.
- `v-for`: hace un bucle para pintar tiles


??? v-list example
    ```vue
    <v-list>
      <v-list-tile
        v-for="item in menuItems"
        :key="item.title"
        :to="item.path">
        <v-list-tile-action>
          <v-icon>{{ item.icon }}</v-icon>
        </v-list-tile-action>
        <v-list-tile-content>{{ item.title }}</v-list-tile-content>
      </v-list-tile>
      <v-list-tile @click="userSignOut" v-if="isAuthenticated">
        <v-list-tile-action>
          <v-icon>exit_to_app</v-icon>
        </v-list-tile-action>
        <v-list-tile-content>Sign Out</v-list-tile-content>
      </v-list-tile>
    </v-list>
    ```
