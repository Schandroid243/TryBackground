<template>
  <div class=" mt-4" style="padding-top: 15px ">
      <b-row class="align-items-center justify-content-center">
        <b-col class=" col-md-12 col-xl-12 col-lg-12 p-0">
        <b-container class="mt-4 mb-4 text-danger h5">
          <!-- <h5 class="text-danger"> <em>{{info}}</em> </h5> -->
        </b-container>
        <div class="m-4 d-flex" style=" justify-content:space-between">
          <h5 class="text-white">Contact : {{contact.firstName}} {{contact.name}} {{contact.lastName}}</h5>
          <nuxt-link class="btn btn-secondary text-decoration-none text-white"
                    :to=" {name: 'profile-add-id', params: {id: contact.id}} ">
                    <b-icon icon="person-plus-fill" width="22px" height="22px"></b-icon>
                    <span class="px-2">Créer</span>
          </nuxt-link>
        </div>
        <b-container fluid class=" text-center" style="padding-top:30px">
          <b-row>
            <b-col cols="12" class="mt-4 mb-4">
          <b-row v-if="loader">
            <b-card class="col-md-12" style="height:100%">
              <b-skeleton animation="throb" width="85%"></b-skeleton>
              <b-skeleton animation="throb" width="55%"></b-skeleton>
              <b-skeleton animation="throb" width="70%"></b-skeleton>
            </b-card>
          </b-row>
          <b-row v-else class="d-flex col-md-12" style="justify-content:space-around" >
            <div class="col-md-6 mt-1" v-for="(profile, i) in listProfile" :key="i">
            <b-card
            class="shadow"
            style=" width:100%; height:100%; border-radius: 20px;">
            <b-container class="align-items-center justify-content-center mt-4">
            <b-row class="d-flex justify-content-center align-items-center text-center">
              <b-col>
                <img src="~/assets/profile.jpeg" style="width:80%; height:100%">
              </b-col>
              <b-col>
                <div>
                <h5 class="text-dark mt-2 mb-2"> {{profile.organization}} </h5>
                <h6 class="text-secondary mt-2 mb-2"> {{profile.title}} </h6>
                <b-container class="d-flex" style="justify-content: space-around;">
                  <nuxt-link style="width:30%; height:100%" class="btn text-decoration-none text-white"
                    :to=" {name: 'profile-edit-id', params: {id: profile.id}} ">
                    <b-icon icon="pencil-square"></b-icon>
                  </nuxt-link>
                  <nuxt-link  style="width:30%; height:100%"  class="btn text-decoration-none text-white"
                    :to=" {name: 'profile-show-id', params: {id: profile.id, contact_id: profile.contact_id}}">
                    <b-icon icon="file-earmark-person"></b-icon>
                  </nuxt-link>
                  <nuxt-link  style="width:30%; height:100%"  class="btn text-decoration-none text-white"
                    :to=" {name: 'profile-delete-id', params: {id: profile.id}}">
                    <b-icon icon="eraser-fill"></b-icon>
                  </nuxt-link>
                </b-container>
                <div>
                  <h6 v-if="contact.paymentStatus" class="text-info mt-2 mb-2">Status : activate</h6>
                  <h6 v-else  class="text-danger mt-2 mb-2">Status : desactivate</h6>
                  <h6 class="text-secondary mx-2 mb-2"><em>Votre abonnement expire le :</em></h6>
                  <h6 class="text-danger mt-2 mb-2">{{ date }} </h6>
                </div>
                <div v-if = "profile.expirationDate !== Date.now">
                  <b-button @click="activation(profile.id)" class="btn text-white">Activate</b-button>
                </div>
              </div>
              </b-col>
              
            </b-row>
            </b-container>
              </b-card>
            </div>
          </b-row>
          </b-col>
          </b-row>
          <b-modal id="modal-1" title="Message" ok-only hide-header-close>
                  <p class="my-4">Votre profile a été activé</p>
          </b-modal>
          <b-modal id="modal-2" title="Message" ok-only hide-header-close>
                  <p class="my-4">Cette carte est encours de validité !</p>
          </b-modal>
        </b-container>
       </b-col>
      </b-row>
  </div>
</template>

<script>
import { Console } from 'console'
  export default {
    data() {
      return {
        id: '',
        date: '',
        time: '',
        profile: {
          contact_id: '',
          title: '',
          organization: '',
          status: false,
          expirationDate: '',
        },
        contact: {
          id: '',
          name: '',
          firstName: '',
          lastName: '',
          paymentStatus: true,
          expirationDate: '',
        },
        loader: false,

        listProfile: [],
        dataProfile: [],
      }
    },
    created() {
      this.init()
      this.getProfile()
      this.getContact()
    },

    mounted() {
      this.init()
      this.getContact()
    },

    methods: {

      init() {
        this.id = this.$route.params.id
        this.token = this.$auth.strategy.token.get()
      },
      creer() {
        this.$router.push({
          name: 'contact'
        })
      },

      getProfile() {

        this.loader = true
        this.$axios.get('profile/AllProfiles/', {
          headers: {
            'Content-Type': 'application/json',
            'Access-Control-Allow-Origin': '*',
            'x-access-token': this.token
          },
        }, {
          withCredentials: true
        }).then((response) => {

          this.dataProfile = response.data
          this.dataProfile.forEach((element) => {
            if(element.contact_id == this.id){
              this.listProfile.push(element)
            }
          });

        }).catch((error) => {
          console.log(error)
          this.info = 'vous êtes hors-connexion'
        }).finally(() => {
          this.loader = false
        })
      },
      activation() {
        this.$axios.put(`contact/activateContact/${this.id}`, {
          headers: {
            'Content-Type': 'application/json',
            'Access-Control-Allow-Origin': '*',
            'x-access-token': this.token
          },
        }, {
          withCredentials: true
        }).then((response) => {
          console.log(response)
          this.$bvModal.show('modal-1')
          this.$router.go(0)

        }).catch((error) => {
          console.log(error)
          this.$bvModal.show('modal-2')
          
          this.info = 'vous êtes hors-connexion'
        }).finally(() => {
          this.loader = false
        })
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
          this.contact = response.data

          var time = Date.parse(this.contact.expirationDate);
          this.date = new Date(time).toUTCString();
          
        }).catch((error) => {
          console.log(error)
          this.info = 'vous êtes hors-connexion'
        }).finally(() => {
          this.loader = false
        })
      },
    }
  }

</script>

<style>

</style>
