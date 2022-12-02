<template>
  <div class="mt-4" style="padding-top:15px">
      <b-row class="align-items-center justify-content-center">
        <div class=" col-md-9 col-xl-9 col-lg-9 text-dark">
          <b-container class="mt-4 mb-4">
            <h3 class="text-white">Créer un nouvel utilisateur</h3>
          </b-container>
          <b-container fluid v-if="loader" class="mt-4 mb-4">
            <b-row>
              <b-card class="col-md-12" height="100">
                <b-skeleton animation="throb" width="85%"></b-skeleton>
                <b-skeleton animation="throb" width="55%"></b-skeleton>
                <b-skeleton animation="throb" width="70%"></b-skeleton>
              </b-card>
            </b-row>
          </b-container>
          <b-card v-else class="col-md-12">
            <b-row>
                <h6 class="text-danger"> {{info}} </h6>
            </b-row>
            <b-row class="mt-1 mb-1">
              <b-col lg="3">
                <h6>Contact</h6>
              </b-col>
              <b-col lg="9">
                <b-form-group>
                  <b-form-input v-model="form.firstName" class="mt-1 mb-1" id="FirstName"
                    placeholder="Entrer votre prénom"></b-form-input>
                  <b-form-input v-model="form.name" class="mt-1 mb-1" id="FirstName" placeholder="Entrer votre nom">
                  </b-form-input>
                  <b-form-input v-model="form.lastName" class="mt-1 mb-1" id="LastName"
                    placeholder="Entrez votre PostNom"></b-form-input>
                    <template>
                      <div class="">
                        <b-form-select v-model="form.role" :options="options"></b-form-select>
                      </div>
                    </template>
                </b-form-group>
              </b-col>
            </b-row>
          </b-card>
          <b-button-group class="mt-4 d-flex align-items-center justify-content-sm-end">
      <b-button @click="submitForm"
      class="btn-info text-white col-md-4 mx-2 shadow">
      Enregistrer
      </b-button>
      <b-button @click="cancel" class="btn-danger text-white col-md-4 mx-2 shadow">Annuler</b-button>
    </b-button-group>
        </div>
      </b-row>
  </div>
</template>

<script>
  export default {
    name: 'User_add',
    data() {
      return {
        form: {
          name: '',
          firstName: '',
          lastName: '',
          role: null,
        },
        options: [
          {value: null, text:'Veuillez choisir votre fonction'}
        ]
      }
    },
    mounted() {
      this.init()
    },
    methods: {
      submitForm() {
        this.$axios.post('contact/addContact', this.form, {
          headers: {
            'Content-Type': 'application/json',
            'Access-Control-Allow-Origin': '*',
            'x-access-token': this.token
          },
        }, {
          withCredentials: true
        }).then((response) => {
          console.log(response)
          this.form = response.data
        }).catch((error) => {
          console.log(error)
        })
        this.$router.push({
          name: 'contact-listContact'
        })
      },
      init() {
        this.token = this.$auth.strategy.token.get()
      },
      cancel() {
        this.$router.go(-1);
      }
    }
  }

</script>
