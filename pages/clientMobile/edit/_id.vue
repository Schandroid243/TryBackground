<template>
    <div class="mt-4" style="padding-top:15px">
        <b-row class="align-items-center justify-content-center">
          <div class=" col-md-9 col-xl-9 col-lg-9 text-dark">
            <b-container class="mt-4 mb-4">
              <h3 class="text-white">Modifier un client mobile</h3>
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
                  <h6>Client</h6>
                </b-col>
                <b-col lg="9">
                  <b-form-group>
                    <b-form-input v-model="form.name" class="mt-1 mb-1" id="name" placeholder="Entrer votre nom">
                    </b-form-input>
                    <b-form-input v-model="form.email" class="mt-1 mb-1" id="LastName"
                      placeholder="Entrez votre email"></b-form-input>
                      <b-form-input v-model="form.password" class="mt-1 mb-1" id="LastName"
                      placeholder="Entrez votre mot de passe"></b-form-input>
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
          id: '',
          form: {
            name: '',
            email: '',
            password: '',
          },
         
        }
      },
      mounted() {
        this.init()
        this.getClientMobileDetail()
      },
      created() {
        this.init()
        this.getClientMobileDetail()
      },
      methods: {
        init() {
            this.token = this.$auth.strategy.token.get()
            this.id = this.$route.params.id
          },
        getClientMobileDetail() {
          this.$axios.get(`clientMobile/clientMobileDetails/${this.id}`, {
            headers: {
              'Content-Type': 'application/json',
              'Access-Control-Allow-Origin': '*',
              'x-access-token': this.token},
          }, {withCredentials: true}).then((response) => {
            console.log(response)
            this.form = response.data
          }).catch((error) => {
            console.log(error)
          })
        },
        submitForm() {
          this.$axios.put('clientMobile/updateClientMobile', this.form, {
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
            name: 'clientMobile-listClientMobile'
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
  