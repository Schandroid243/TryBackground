<template>
    <b-container>
      <b-modal v-model="modalShow"
               hide-footer
               no-close-on-backdrop>
          <template #modal-header>
          <b-col>
              <b-row class="d-block text-center">
                  <h5>Voulez-vous supprimer le client ?</h5>
              </b-row>
          </b-col>
          </template>
  
      <b-container class="d-flex justify-content-center col-md-6"
      style="gap:20px">
      <b-button class="mt-3 btn-info" block @click="cancel">Annuler</b-button>
      <b-button class="mt-3 btn-danger" block @click="deleteClient">Supprimer</b-button>
      </b-container>
      </b-modal>
    </b-container>
  </template>
  
  <script>
  export default {
  data() {
      return {
          form: {
              id: '',
             name: '',
             email: '',
  
          },
          token: '',
      }
  },
  created() {
      this.init()
      this.modalShow()
  
  },
  methods: {
      modalShow() {
          this.modalShow = true
      },
      init() {
            this.token = this.$auth.strategy.token.get()
            this.id = this.$route.params.id
          },
          deleteClient() {
            // if(!confirm('voulez-vous supprimer le user ?')) {
            //   return
            // }
            this.$axios.delete(`clientMobile/deleteClientMobile/${this.id}`, {
              headers: {
                'Content-Type': 'application/json',
                'Access-Control-Allow-Origin': '*',
                'x-access-token': this.token},
            }, {withCredentials: true}).then((response) => {
              this.$bvToast.toast(`le client ${this.form.name} a été supprimé avec succès !`, {
              title: 'Message Admin',
              variant: 'danger',
              autoHideDelay: 5000,
              appendToast: true });
  
            })
            this.$router.push({
              name: 'clientMobile-listClientMobile'
            })
          },
          cancel() {
            this.$router.push({
              name: 'clientMobile-listClientMobile'
            })
          }
  }
  }
  </script>
  
  <style>
  
  </style>
  