<template>
  <div class="mt-4" style="padding-top:15px">
      <b-row class="align-items-center justify-content-center">
        <div class=" col-md-9 col-xl-9 col-lg-9 text-dark">
          <b-container class="mt-4 mb-4">
            <h3 class="text-white">Créer un nouveau contact pour le client :</h3>
          </b-container>
          <b-container fluid v-if="loader" class="mt-4 mb-4">
            <b-row>
              <b-card class="col-md-9" height="100">
                <b-skeleton animation="throb" width="85%"></b-skeleton>
                <b-skeleton animation="throb" width="55%"></b-skeleton>
                <b-skeleton animation="throb" width="70%"></b-skeleton>
              </b-card>
            </b-row>
          </b-container>
          <b-container v-else>
            <form @submit.prevent="submitForm">
             
          <b-card  class="col-md-12">
            <b-row>
                <h6 class="text-danger"> {{info}} </h6>
            </b-row>
            <b-row class="mt-1 mb-1">
              <b-col lg="3">
                <h6>Contact</h6>
              </b-col>
              <b-col lg="9">
                <b-form-group>
                  <b-form-input required v-model="form.firstName" class="mt-1 mb-1" id="FirstName"
                    placeholder="Entrer votre prénom"></b-form-input>
                  <b-form-input v-model="form.name" class="mt-1 mb-1" id="FirstName" placeholder="Entrer votre nom">
                  </b-form-input>
                  <b-form-input v-model="form.lastName" class="mt-1 mb-1" id="LastName"
                    placeholder="Entrez votre PostNom"></b-form-input>
                </b-form-group>
              </b-col>
            </b-row>
          </b-card>
          <b-button-group class="mt-4 d-flex align-items-center justify-content-sm-end">
            <b-spinner v-if="loader" variant="primary"></b-spinner>
            <b-button type="submit"
            class="btn-info text-white col-md-4 mx-2 shadow">
            Enregistrer
            </b-button>
      <b-button @click="cancel" class="btn-danger text-white col-md-4 mx-2 shadow">Annuler</b-button>
    </b-button-group>
            </form>
       
          </b-container>
          
        </div>
      </b-row>
  </div>
</template>

<script>
  export default {
    name: 'ContactCard',
    data() {
      return {
        id: "",
        loader: false,
        info: '',
        form: {
          name: '',
          firstName: '',
          lastName: '',
          clientmobile_id: '',
          user_id: '',
        },
        currentUser: {
          name: '',
          role: '',
          id: ''
        },
        options: [
              {value: null, text: 'Veuillez choisir votre un client'},
            ],
            client : {
              id: '',
              name: ''
            },
            user : {
              id: '',
              name: '',
            },
            userList: [],
      }
    },
    // computed: {
    //   myClients() {
    //     return this.clientMobileList.map(client => ({value: client.id, text: client.name}))
    //   },
    //   myUsers() {
    //     return this.userList.map(user => ({value: user.id, text: user.name}))
    //   }
    // },
    mounted() {
      this.init()
      this.getClientMobile()
      this.getUsers() 
    },
    methods: {
      getClientMobile() {
        this.loader = true
  
        this.$axios.get(`clientMobile/clientMobileDetails/${this.id}`, {
          headers: {
            'Content-Type': 'application/json',
            'Access-Control-Allow-Origin': '*',
            'x-access-token': this.token},
        }, {withCredentials: true}).then((response) => {
          console.log(response);
          return this.client = response.data
        }).catch((error) => {
          console.log(error)
        }).finally(() => {
          this.loader = false
        })
      },
      getUsers() {
      this.loader = true

      this.$axios.get('auth/userDetails', {
              headers: {
                'Content-Type': 'application/json',
                'Access-Control-Allow-Origin': '*',
                'x-access-token': this.token},
            }, {withCredentials: true}).then((response) => {
              console.log(response)
              console.log(this.currentUser)
              return this.currentUser = response.data.data
            }).catch((error) => {
              console.log(error)
            }).finally(() => {
        this.loader = false
      })
    },
      submitForm() {
        this.form.clientmobile_id = this.id
        this.form.user_id = this.currentUser.id
        this.$axios.post('contact/addContact', this.form, {
          headers: {
            'Content-Type': 'application/json',
            'Access-Control-Allow-Origin': '*',
            'x-access-token': this.token
          },
        }, {
          withCredentials: true
        }).then((response) => {
          this.$bvToast.toast(`le contact ${this.form.firstName} a été créé avec succès !`, {
              title: 'Message Admin',
              variant: 'primary',
              autoHideDelay: 5000,
              appendToast: true });

          this.$router.push({
            name: 'contact-listContact'

          })
        }).catch((error) => {
          this.$bvToast.toast('une erreur est survenue, veuillez réessayer !', {
              title: 'Message Admin',
              variant: 'danger',
              autoHideDelay: 5000,
              appendToast: true });

        })
      },
      init() {
        this.token = this.$auth.strategy.token.get()
        this.id = this.$route.params.id
      },
      cancel() {
        this.$router.push({
            name: 'contact-listContact'
          })
      }
    }
  }

</script>

<style>

</style>
