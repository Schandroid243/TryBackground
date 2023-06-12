<template>
  <b-container>
    <b-row class="flex no-wrap min-vh-100 justify-content-center align-items-center">
      <b-col class="col-auto col-md-6 col-xl-5 col-sm-5">
        <section class="texte display-3 text-white">Welcome Back to DaureCard</section>
      </b-col>
      <b-col class=" col-auto col-md-6 col-xl-5 col-sm-5 text-center">
        <b-card style="max-width: 50rem " title="LOGIN" class=" loader justify-content-center text-white">
          <form @submit.prevent="userLogin">
            <template>
            <b-container class="mt-4 mb-4 text-danger h5">
             <em>{{info}}</em>
            </b-container>

            <b-form-input

            class="mt-4 mb-4"
            type="text"
            size="xl"
            v-model="login.email"
            required
            placeholder="Entrez votre email">
            </b-form-input>
            <b-form-input

              class="mt-4 mb-4"
              type="password"
              size="xl"
              v-model="login.password"
              required
              placeholder="Entrez votre mot de passe">
            </b-form-input>

            <div class="d-flex" style="gap:20px">
              <b-spinner v-if="loader" variant="primary"></b-spinner>
              <b-button type="submit" class="btn-info">
                Connexion
              </b-button>
              <nuxt-link to="register"><em>Créer compte ?</em></nuxt-link>
            </div>
          </template>
          </form>
        </b-card>
      </b-col>
    </b-row>

  </b-container>
</template>

<script>
import Vue from 'vue'
import { BootstrapVueIcons } from 'bootstrap-vue'
Vue.use(BootstrapVueIcons)

  export default {
    layout: 'login',

    data() {
      return {
        login: {
          email: "",
          password: "",

        },
        info: '',
        userID: '',
        loader: false
      }
    },
    methods: {
      // init() {
      // },

      async userLogin() {
        this.loader = true
        try {
          let response = await this.$auth.loginWith('local', {
            data: this.login
          })

          var token = response.data.data.token
          var refreshToken = response.data.response.refreshToken
          this.userID = response.data.data.userId;
          this.$auth.strategy.token.set(token);
          //this.$auth.strategy.refreshToken.set(refreshToken);
          //console.log(this.$auth.strategy.token.get())
          //this.$auth.setRefreshToken('local', refreshToken)
          await this.$auth.setUser({
            email: response.data.data.email,
            password: this.login.password
          })
          await this.$router.replace({
            name: 'home'
          })

        } catch (err) {
          this.info = 'Email ou mot de passe incorrect !'
        } finally{
          this.loader = false
        }
      },
    }
  }

</script>

<style>

</style>
