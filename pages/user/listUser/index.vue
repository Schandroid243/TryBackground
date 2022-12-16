<template>
  <b-container fluid>
    <b-row class="mt-4 align-items-center justify-content-center"
    style="padding-top:15px">
      <b-col class=" col-md-12 col-xl-12 col-lg-12 text-dark">
      <div class="mt-4 mb-4 d-flex" style=" justify-content:space-between ; ">
      <h4 class="text-white">Liste des utilisateurs</h4>
      </div>
      <b-container class="mt-4 mb-4 col-md-12 col-xl-12 col-sm-12">
        <b-row>
          <b-col v-if="loader" class="mt-4 mb-4">
              <b-row>
              <b-card class="col-md-12" height="100">
                <b-skeleton animation="throb" width="85%"></b-skeleton>
                <b-skeleton animation="throb" width="55%"></b-skeleton>
                <b-skeleton animation="throb" width="70%"></b-skeleton>
              </b-card>
              </b-row>
            </b-col>

          <b-table v-else class="table-light shadow text-center" striped hover :items="userList" :fields="fields">
            <col v-for="(user, i) in userList" :key="i">
              <template #cell(Nom)="user"> {{user.item.name}} </template>
              <template #cell(Email)="user"> {{user.item.email}} </template>

              <template #cell(Action)="user">
                <b-row class="d-flex col-md-12 align-items-center justify-content-center" style="gap:10px">
                  <nuxt-link :to=" {name: 'user-edit-id', params: {id: user.item.id}}">
                    <b-button v-if="currentUser == user.item.email"
                      variant="secondary" >
                     <b-icon icon="pencil-square"></b-icon>
                    </b-button>

                  </nuxt-link>
                  <nuxt-link :to=" {name: 'user-delete-id', params: {id: user.item.id}}">
                    <b-button v-if="checkUser"
                      variant="danger" >
                     <b-icon icon="eraser-fill"></b-icon>
                    </b-button>
                  </nuxt-link>
                </b-row>



              </template>
          </b-table>
        </b-row>

      </b-container>
    </b-col>
    </b-row>
  </b-container>
</template>
<script>
import { info } from 'console'


export default {

  data() {
    return {
      fields: ['name', 'email', 'Action'],
      userList: [],
      token: '',
      refreshToken: '',
      currentUser: '',
      checkUser: false,
      loader: false
      }
    },
  created() {
    this.init()
    this.getUserDetail()
    this.getUsers()

  },
  mounted () {
    this.init()
    this.getUserDetail()
    this.getUsers()
  },

  methods: {
    init() {
      this.token = this.$auth.strategy.token.get()
    },
    getUsers() {
      this.loader = true

      this.$axios.get('auth/users', {
        headers: {
          'Content-Type': 'application/json',
          'Access-Control-Allow-Origin': '*',
          'x-access-token': this.token},
      }, {withCredentials: true}).then((response) => {
        return this.userList = response.data
      }).catch((error) => {
        console.log(error)
      }).finally(() => {
        this.loader = false
      })
    },
    getUserDetail() {
        this.$axios.get('auth/userDetails', {
          headers: {
            'Content-Type': 'application/json',
            'Access-Control-Allow-Origin': '*',
            'x-access-token': this.token},
        }, {withCredentials: true}).then((response) => {
          console.log(response)
          this.currentUser = response.data.data.email
          if(response.data.data.role == 'Admin') {
            this.checkUser = true
          } else {
            this.checkUser = false
          }
          console.log(this.currentUser)
        }).catch((error) => {
          console.log(error)
        })
      },

  }
}
</script>
<style scoped>

</style>
