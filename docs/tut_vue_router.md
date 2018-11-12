# Detalles: `src/router.js`
## Código
??? "./src/router.js"
    ```js
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

## Importaciones
Se importan:

```js
import Vue from 'vue'
import Router from 'vue-router'
import firebase from 'firebase'
```

las librerías:

- vue
- vue-router
- firebase

## Rutas
Definimos las rutas:

```js
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
```

en donde `routerOptions` es una lista de objetos en los que se indica la ruta y el componente asociado (por nombre). Como vemos, para `home`, se requiere que el usuario esté autentificado. Puede ser deseable redirigir a los usuarios que quieran acceder a rutas protegidas.

Vemos que se define la función `component`, que es la que importa el fichero `.vue`.


## Usar el router
Indicamos a *Vue* que use *Router*. Por otro lado, se crea una instancia configurada en modo `history`:

```js
Vue.use(Router)

const router = new Router({
  mode: 'history',
  routes
})
```

## Rutas protegidas
Vimos con anterioridad que queríamos proteger la siguiente ruta:

```js
  { path: '/home', component: 'Home', meta: { requiresAuth: true } },
```

Para posibilitarlo, definimos la siguiente función (guard function) que se ejecutará antes de que accedamos a una ruta:

```js
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

Como vemos, la siguiente línea comprueva si la ruta a la que vamos tiene un valor `meta.requiresAuth=true`:

```js
const requiresAuth = to.matched.some(record => record.meta.requiresAuth)
```

La siguiente línea devuelve el usuario actual:

```js
const isAuthenticated = firebase.auth().currentUser
```

El siguiente código nos envía a la página `/signin` cuando la ruta requiera autorización y no estemos logeados. Nos llevará a la página objetivo en caso contrario.

Finalmente se exporta la instacia `router`.


Sin embargo esto no funcionará:

Si entramos en la aplicación y hacemos "sign in", nos redirigirá a `/home`. Si volvemos a cargar la página, nos enviaría a `/signin`. ¿Por qué? Porque cuando `beforeEach` ejecuta `firebase.auth().currentUser` obtenemos `null`. Esto ocurre porque durante la ejecución de `beforeEach`, nuestro Firebase `onAuthStateChanged` no ha resuelto el estado del usario todavía.

La solución consiste en ejecutar la función `beforeEach` después de que `onAuthStateChanged` haya resuelto el usuario.
