<template>
  <div>
   <b-container fluid class="mt-4">
   <b-row class="align-items-center justify-content-center">
     <b-col class="mt-4 col-md-12 col-xl-12 d-flex" style="justify-content: space-between;
            padding-top:15px">
         <div>
           <b-card class="shadow col-md-12 col-xl-12 col-sm-12" style=" height:100px">
           <b-container class=" text-info d-flex align-items-center text-info"
           style="font-size:36pt; justify-content: space-between;">
           <b-icon icon="people-fill" variant="info" width="60" height="60"></b-icon>
           <h2>Contacts enregistrés</h2>
           <nuxt-link class="text-decoration-none" :to="{name: 'contact-listContact'}">
           <b-spinner v-if="loader" label="large spinner" variant="info"></b-spinner>
           <b-badge v-else variant="info" pill>
             {{contactNbr}}
           </b-badge>
         </nuxt-link>
           </b-container>

           </b-card>
             <b-carousel id="carousel-fade"
             class="text-center col-md-12 col-xl-12 col-sm-12"
               style="text-shadow: 0px 0px 2px #000;"
               fade indicators
               img-width="400"
               img-height="100">
             <b-carousel-slide caption="Enregistrez un contact"
             img-src= '~/assets/info.jpg'>
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
       <b-card style="height:68%;">
         <img src="~/assets/aurtechDaurecard.jpg" style="height:100%; width:100%">
       </b-card>
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
         console.log(response)
         return this.listContact = response.data

       }).catch((error) => {
         console.log(error)
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
