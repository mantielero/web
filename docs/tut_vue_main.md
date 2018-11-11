# Detalles: `src/main.js`
??? ./src/main.js
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
