<template>
  <div>
    <div id="firebaseui-auth-container"></div> 
  </div>
</template>

<script>

import * as firebase from "firebase/app"
import * as firebaseui from 'firebaseui'

export default {
  name: 'FirebaseAuthentication',
  props: {
  },
  data: () => ({

    // This configuration can be copy and pasted from the Settings page of your
    // Firebase project in the web dashboard. Each Firebase project app has their own configuration.
    firebaseConfig: {
      apiKey: "someapikey",
      authDomain: "some-app-domain.firebaseapp.com",
      databaseURL: "https://some-database-url.firebaseio.com",
      projectId: "some-project-id",
      storageBucket: "some-store-bucket.appspot.com",
      messagingSenderId: "somesenderid",
      appId: "someappid"
    },
    // https://firebaseopensource.com/projects/firebase/firebaseui-web/#configuration
    firebaseConfigUI: {
      signInSuccessUrl: '<url-to-redirect-to-on-success>',
      signInOptions: [
        // Leave the lines as is for the providers you want to offer your users.
        // These need to be independently enabled through the web console.
        firebase.auth.GoogleAuthProvider.PROVIDER_ID,
        firebase.auth.FacebookAuthProvider.PROVIDER_ID,
        firebase.auth.TwitterAuthProvider.PROVIDER_ID,
        firebase.auth.GithubAuthProvider.PROVIDER_ID,
        firebase.auth.EmailAuthProvider.PROVIDER_ID,
        firebase.auth.PhoneAuthProvider.PROVIDER_ID,
        firebaseui.auth.AnonymousAuthProvider.PROVIDER_ID
      ],
      // tosUrl and privacyPolicyUrl accept either url string or a callback
      // function.
      // Terms of service url/callback.
      tosUrl: '<your-tos-url>',
      // Privacy policy url/callback.
      privacyPolicyUrl: function() {
        window.location.assign('<your-privacy-policy-url>');
      }
    }
  }),
  computed: {

  },
  methods: {
    signInAttempSuccessful(authResult, redirectUrl) {
      alert('Sign in successfull. See dev console for authorization response')
      console.log('authResult', authResult)
      console.log('redirectUrl', redirectUrl)
      // Do what you wish with authResult... save to session, cookie, etc.
    },
    // Note, bad credentials is not a sign-in failure
    signInAttempFailure(error) {
      alert('Sign in failed. See dev console for error response')
      console.log('error', error)
    },
    firebaseUiShown: function() {
      console.log('Firebase UI widget rendered')
    }
  },
  watch: {

  },
  mounted() {
    // Note, if running your application locally and relying on Vue CLI's hot reload
    // (npm run serve, $ vue-cli-service serve) you will get "Firebase App already exists" errors.
    // You'll need to reload the page manually to initialize FirebaseUI

    // Set callback functions to Firebase UI config.
    // These can be defined in Vue data but I prefer to assign them to Vue methods.
    this.firebaseConfigUI.callbacks = {
      signInSuccessWithAuthResult: this.signInAttempSuccessful,
      signInFailure: this.signInAttempFailure,
      uiShown: this.firebaseUiShown,
    }  
    // Initialize Firebase app
    var app = firebase.initializeApp(this.firebaseConfig)
    // Initialize the FirebaseUI Widget using the Firebase app.
    var ui = new firebaseui.auth.AuthUI(app.auth())
    // The start method will wait until the DOM is loaded.
    ui.start('#firebaseui-auth-container', this.firebaseConfigUI)
  }
}
</script>