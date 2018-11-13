# Firebase - Almacenamiento
## Objetivo
Crear un formulario y guardarlo online con Firebase.

Supondremos que partimos de el código listo para la autentificación.



## Vuefire
Vuefire nos facilita el trabajar con Firebase.

### Instalación
Se instala mediante:

```bash
npm i --save vuefire
```

### Configuración
En `./src/main.js`:

??? "Modificaciones de `./src/main.js`"
    ```js hl_lines="7 9 16 18 29 30 31"
    import Vue from 'vue'
    import './plugins/vuetify'
    import App from './App.vue'
    import router from './router'
    import store from './store'
    import firebase from 'firebase'
    import Vuefire from 'vuefire'

    var firebaseApp = firebase.initializeApp({
      apiKey: "AIzaSyBZXlv0CUZBdPWvyzzi2eaBlXT764hoIDM",
      authDomain: "prueba-28ec5.firebaseapp.com",
      databaseURL: "https://prueba-28ec5.firebaseio.com",
      projectId: "prueba-28ec5"
    })

    var db = firebaseApp.database();

    Vue.use(Vuefire)

    /* eslint-disable */
    Vue.config.productionTip = false

    const unsubscribe = firebase.auth()
      .onAuthStateChanged((firebaseUser) => {
        new Vue({
          //el: '#app',
          router,
          store,
          firebase: {
             agenda: db.ref('agenda').orderByChild('created_at')
          },
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

Hemos creado una referencia a la base de datos.

También importamos *vuefire*. Vuefire exige que definamos el objeto `firebase` en la instancia de `Vue`.

### Template
Lo primero es crear un borrador de la interfaz gráfica que queremos (algo sencillo: nombre y apellido):

??? "AddContact.vue"
    ```vue hl_lines="45 46 47 48 49 50 51 52 53 54"
    <template>
      <v-container fluid>
        <v-layout row wrap>
          <v-flex xs12 class="text-xs-center" mt-5>
            <h1>Add Contact</h1>
          </v-flex>
          <v-flex xs12 sm6 offset-sm3 mt-3>
            <form @submit.prevent="sendContact">
              <v-layout column>
                <v-flex>
                  <v-alert type="error" dismissible v-model="alert">
                    {{ error }}
                  </v-alert>
                </v-flex>
                <v-flex>
                  <v-text-field
                    name="name"
                    label="Name"
                    id="name"
                    v-model="name"
                    required></v-text-field>
                </v-flex>
                <v-flex>
                  <v-text-field
                    name="surname"
                    label="Surname"
                    id="surname"
                    v-model="surname"
                    required></v-text-field>
                </v-flex>
                <v-flex class="text-xs-center" mt-5>
                  <v-btn color="primary" type="submit">Submit</v-btn>
                </v-flex>
              </v-layout>
            </form>
          </v-flex>
        </v-layout>
      </v-container>
    </template>

    <script>
    export default {
      data: () => ({

        sendContact () {
          this.$root.$firebaseRefs.agenda.push(
            {
              // 'url': this.catUrl,
              'name': this.name,
              'surname': this.surname,
              'created_at': -1 * new Date().getTime()
            })
            .then(this.$router.push('/'))
        }

      })

    }
    </script>
    ```


# Firestore
[Firestore](https://savvyapps.com/blog/definitive-guide-building-web-app-vuejs-firebase)
