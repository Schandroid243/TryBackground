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

          <b-table v-else class="table-light shadow text-left" striped hover :items="userList" :fields="fields">
            <col v-for="(user, i) in userList" :key="i">
              <template #cell(Nom)="user"> {{user.item.name}} </template>
              <template #cell(Email)="user"> {{user.item.email}} </template>

              <template #cell(Action)="user">
              
                  <b-row class="d-flex col-md-12 align-items-center justify-content-start p-0" style="gap:10px">
                            <div>
                              <b-dropdown id="dropdown-action" text="actions" class="m-2">
                                <b-dropdown-item-button class="">
                                  <nuxt-link class="d-flex text-decoration-none align-items-center justify-content-center" :to=" {name: 'user-edit-id', params: {id: user.item.id}}">
                                    <b-icon icon="pencil-square"></b-icon>
                                    <p class="m-2">Editer</p>
                                  </nuxt-link>
                                </b-dropdown-item-button>
  
                                <b-dropdown-item-button class="">
                                  <nuxt-link class="d-flex text-decoration-none align-items-center justify-content-center" :to=" {name: 'user-delete-id', params: {id: user.item.id}}">
                                    <b-icon icon="eraser-fill"></b-icon>
                                    <p class="m-2">Effacer</p>
                                  </nuxt-link>
                                </b-dropdown-item-button>
                              </b-dropdown>
                            </div>
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
      fields: [' ','name', 'email', 'Action'],
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
        
          this.currentUser = response.data.data.email
          if(response.data.data.role == 'Admin') {
            this.checkUser = true
          } else {
            this.checkUser = false
          }
        
        }).catch((error) => {
        
        })
      },

  }
}
</script>
<style scoped>

</style>
