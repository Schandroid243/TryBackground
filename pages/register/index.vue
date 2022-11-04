<template>
  <b-container fluid class="justify-content-center align-items-center">
    <b-row class=" min-vh-100 align-items-center justify-content-center">
    <b-form @submit.prevent="submitForm" class="col-md-6 col-xl-5">
    <b-card class="loader col-md-12 justify-content-center">
      <b-card-title class=" text-center mt-4 mb-4 text-white">SIGN UP</b-card-title>
      <b-form-input
      required
      v-model="form.name"
      class="mt-2 mb-2"
      placeholder="Entrer votre nom">
      </b-form-input>

      <b-form-input
      required
      v-model="form.email"
      class="mt-2 mb-2"
      placeholder="Adresse email">
      </b-form-input>

      <b-form-input
      required
      v-model="form.password"
      class="mt-2 mb-2"
      type="password"
      placeholder="Votre mot de passe">
      </b-form-input>
      <p class="text-danger"> {{info}} </p>
      <b-container class="mt-4 mb-4 justify-content-center d-flex" style="gap: 5px">
      <b-button type="submit" class="btn-info">Enregister</b-button>
      <b-button @click="cancel" class="btn-danger">Annuler</b-button>
      </b-container>
  </b-card>
    </b-form>
    </b-row>
  </b-container>
</template>

<script>

export default {
  layout: 'login',
    data() {
        return {
            form : {
            name: '',
            email: '',
            password: '',
            token:''
            },
            info: ''

        }
    },
    methods : {
      submitForm() {
        this.$axios.post('auth/signUp',this.form).then((response) => {
          console.log(response)
        }).catch((error) => {
          console.log({"message": error.message})
        })
        if (this.form.password.length < 6) {
            this.info = 'Mot de passe : plus de 6 caractères'
          } else {
            this.$router.go(-1)
          }
      },
      cancel() {
        this.$router.go(-1)
      },
    }

}
</script>

<style>

</style>
