<template>
  <div class="mt-4" style="padding-top:15px">
      <b-row class="align-items-center justify-content-center">
        <div class=" col-md-9 col-xl-9 col-lg-9 text-dark">
          <b-container fluid class="mt-4 mb-4 p-0">
            <h3 class="text-white">Editez contact</h3>
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
              </b-form-group>

            </b-col>
          </b-row>
          </b-card>
          <b-button-group class="d-flex mt-4 mb-4  d-flex align-items-center justify-content-md-end">
            <b-button @click="submitForm" class="btn-secondary text-white col-md-4 mx-2">
              <b-icon icon="save-fill" width="11px" height="11px"></b-icon>
              Enregistrer
            </b-button>
            <b-button @click="cancel" class="btn-danger text-white col-md-4 mx-2">Annuler</b-button>
          </b-button-group>
        </div>
      </b-row>
  </div>
</template>

<script>
  export default {
    name: 'EditerContact',
    data() {

      return {
        id: "",
        form: {
          firstName: '',
          name: '',
          lastName: '',
        },
        token: '',
        info: '',
        loader: false

      }
    },
    created() {
      this.init()
      this.getContact()
    },
    methods: {
      init() {
        this.id = this.$route.params.id
        this.token = this.$auth.strategy.token.get()
      },
      getContact() {
        this.loader = true
        this.$axios.get(`contact/contactDetail/${this.id}`, {
          headers: {
            'Content-Type': 'application/json',
            'Access-Control-Allow-Origin': '*',
            'x-access-token': this.token
          },
        }, {
          withCredentials: true
        }).then((response) => {
          this.form = response.data
        }).catch((error) => {
          this.info = 'vous êtes hors-connexion'
        }).finally(() => {
          this.loader = false
        })
      },
      cancel() {
        this.$router.go(-1)
      },
      submitForm() {
        this.$axios.put(`contact/updateContact/${this.id}`, this.form, {
          headers: {
            'Content-Type': 'application/json',
            'Access-Control-Allow-Origin': '*',
            'x-access-token': this.token
          },
        }, {
          withCredentials: true
        }).then((response) => {
          this.$router.go(-1)
        }).catch((error) => {
        })

      },
    }

  }

</script>

<style>

</style>
