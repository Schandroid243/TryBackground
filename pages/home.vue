<template>
  <div>
   <b-container fluid class="mt-4 p-0">
   <b-row class="align-items-center justify-content-center">
     <b-col class="mt-4 p-0 col-md-12 col-xl-12" style="justify-content: space-between;
            padding-top:15px">

        <div class="col-md-12 col-xl-12 col-sm-12">
              <b-card class="shadow col-md-12 col-xl-12 col-sm-12 mb-4" style=" height:auto">
                <b-row class=" text-info text-center d-flex justify-content-between align-items-center  text-info"
                style="font-size:20pt;">
                <b-icon icon="people-fill" variant="info" width="120" height="100"></b-icon>
                <h5 style="font-size: 2rem;">Contacts enregistrés</h5>
                <nuxt-link class="text-decoration-none" :to="{name: 'contact-listContact'}">

                <b-spinner v-if="loader" label="large spinner" variant="info"></b-spinner>
                <b-container class="bg-info shadow" v-else style="border-radius: 100%; width: 80px; height: 80px;">
                  <h2 class="text-white text-center pt-3">{{contactNbr}}</h2>
                </b-container>
                    
                  </nuxt-link>
                </b-row>
              </b-card>
                <b-carousel id="carousel-fade"
                class="text-center mb-4"
                  style="text-shadow: 0px 0px 2px #000;"
                  fade indicators
                  img-width="1080"
                  img-height="300">
                <b-carousel-slide caption="Enregistrez un contact"
                img-src='~/assets/info.jpg'>
                  <p>Utilisez la barre de navigation à gauche pour créer
                    un contact
                  </p>
                </b-carousel-slide>
                <b-carousel-slide caption="Touches d'actions"
                img-src='~/assets/info.jpg'>
                <p>Les touches d'actions vous permettent de faire les
                  opérations d'éditon, de visualisation ou de suppression d'un contact/Utilisateur
                </p>
                </b-carousel-slide>
                <b-carousel-slide caption="Utilisateur"
                img-src='~/assets/info.jpg'>
                <p>Voir les utilisateurs pouvant se connecter à l'application
                  et en supprimer d'autres.
                </p>
                </b-carousel-slide>
              </b-carousel>
              </div>
     </b-col>
   </b-row>

   <b-row class="align-items-center justify-content-center">
     <b-col class="col-md-12 col-xl-12 col-sm-12 ">
       
         <b-img src="~/assets/daurecardFond.jpg" style="height:100%; width:100%"></b-img>
      
     </b-col>
   </b-row>
   </b-container>
   </div>

</template>

<script>
 export default {

   layout: 'default',
   name: 'home',

   data() {
     return {
       middleware: 'auth',
       listContact: [],
       loader: false
     }
   },
   computed: {
     contactNbr() {
       return this.listContact.length
     }
   },
   async mounted() {
     this.getContacts()
   },
   methods: {
     getContacts() {
       this.loader = true
       this.$axios.get('contact/allContacts', {
         headers: {
           'Content-Type': 'application/json',
           'Access-Control-Allow-Origin': '*',
           'x-access-token': this.token
         },
       }, {
         withCredentials: true
       }).then((response) => {
         return this.listContact = response.data

       }).catch((error) => {
   
       }).finally(() => {
         this.loader = false
       })
     },
   }
 }

</script>
<style>

</style>

</style>
