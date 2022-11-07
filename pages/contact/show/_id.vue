<template>
  <div class=" mt-4" style="padding-top: 15px ">
      <b-row class="align-items-center justify-content-center">
        <b-col class=" col-md-12 col-xl-12 col-lg-12 p-0">
        <b-container class="mt-4 mb-4 text-danger h5">
          <!-- <h5 class="text-danger"> <em>{{info}}</em> </h5> -->
        </b-container>
        <div class="d-flex" style=" justify-content:space-between">
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
            <div class="col-md-4 mt-1" v-for="(profile, i) in listProfile" :key="i">
            <b-card
            class="shadow "
            style=" width:100%; height:230px">
            <b-container class="align-items-center mt-4">
            <b-row class="d-flex justify-content-center">
              <img src="~/assets/profile.jpeg" style="width:30%; height:100%">
              <div>
                <h5 class="text-dark mt-2 mb-2"> {{profile.organization}} </h5>
                <h6 class="text-danger mt-2 mb-2"> {{profile.title}} </h6>
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
              </div>
            </b-row>
            </b-container>
              </b-card>
            </div>
          </b-row>
          </b-col>
          </b-row>
        </b-container>
       </b-col>
      </b-row>
  </div>
</template>

<script>
  export default {
    data() {
      return {
        id: '',
        profile: {
          contact_id: '',
          title: '',
          organization: '',
        },
        contact: {
          id: '',
          name: '',
          firstName: '',
          lastName: ''
        },
        loader: false,

        listProfile: []
      }
    },
    mounted() {
      this.init()
      this.getProfile()
      this.getContact()
    },
    created() {
      this.init()
      this.getProfile()
      this.getContact()
    },

    methods: {

      init() {
        this.id = this.$route.params.id
        this.token = this.$auth.strategy.token.get()
        console.log('this is id: ' + this.id)
      },
      creer() {
        this.$router.push({
          name: 'contact'
        })
      },

      async getProfile() {

        this.loader = true
        this.$axios.get(`contact/allContactProfiles/${this.id}`, {
          headers: {
            'Content-Type': 'application/json',
            'Access-Control-Allow-Origin': '*',
            'x-access-token': this.token
          },
        }, {
          withCredentials: true
        }).then((response) => {
          console.log(response)
          this.listProfile =  response.data[0].profiles
        }).catch((error) => {
          console.log(error)
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
          console.log(response)
          this.contact = response.data

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
