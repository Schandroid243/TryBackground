<template>
    <div class="mt-4">
      <b-row class="align-items-center justify-content-center">
        <div class=" col-md-12 col-xl-12 col-lg-12 text-dark">
          <b-container class="mt-4 mb-4 text-danger h5">
            <h5 class="text-danger"> <em>{{info}}</em> </h5>
          </b-container>
          <div class="d-flex"
               style="justify-content:space-between;
                padding-top: 15px;">
            <h4 class="text-white">Liste des clients mobiles</h4>
            <nuxt-link class="text-decoration-none" :to="{name: 'clientMobile-add'}">
              <b-button class="btn btn-secondary shadow">
                <b-icon icon="person-plus-fill" width="22px" height="22px"></b-icon>
                <span class="px-2">Créer</span>
              </b-button>
            </nuxt-link>
          </div>
        <b-container fluid class="mt-4 mb-4 col-md-12 col-xl-12 col-sm-12">
          <b-row>
            <form class="justify-content-center mt-4 mb-4">
                <input size="md" class="form-control" type="search" v-model="filter"
                  placeholder="Recherchez client...">
              </form>
              <template>
                <b-container v-if="loader" class="mt-4 mb-4">
                <b-row>
                <b-card class="col-md-12" height="100">
                  <b-skeleton animation="throb" width="85%"></b-skeleton>
                  <b-skeleton animation="throb" width="55%"></b-skeleton>
                  <b-skeleton animation="throb" width="70%"></b-skeleton>
                </b-card>
                </b-row>
              </b-container>
              
            <b-table v-else class="table-light shadow text-left" striped hover :items="clientMobileList" :fields="fields"
            :filter="filter" :per-page="perPage" :current-page="currentPage" small>
              <col v-for="(client, i) in clientMobileList" :key="i">
                <template #cell(Nom)="client"> {{client.item.name}} </template>
                <template #cell(Email)="client"> {{client.item.email}} </template>
  
                <template #cell(Action)="client">
                  <b-row class="d-flex col-md-12 align-items-center justify-content-start" style="gap:10px">
                    <nuxt-link :to=" {name: 'clientMobile-edit-id', params: {id: client.item.id}}">
                      <b-button variant="secondary" >
                       <b-icon icon="pencil-square"></b-icon>
                      </b-button>
                    </nuxt-link>
                    <nuxt-link :to=" {name: 'clientMobile-delete-id', params: {id: client.item.id}}">
                      <b-button variant="danger" >
                       <b-icon icon="eraser-fill"></b-icon>
                      </b-button>
                    </nuxt-link>
                  </b-row>
                </template>
            </b-table>
            <div class="overflow-auto">
                  <b-pagination v-model="currentPage" pills :total-rows="rows" :per-page="perPage"
                    aria-controls="table"></b-pagination>
                </div>
              </template>
          </b-row>
        </b-container>
      </div>
      </b-row>
    </div>
  </template>
  <script>
  import { info } from 'console'
  
  
  export default {
  
    data() {
      return {
        perPage: 10,
        currentPage: 1,
        filter: '',
        fields: ['   ','name', 'email', 'Action'],
        clientMobileList: [],
        token: '',
        refreshToken: '',
        currentUser: '',
        checkUser: false,
        loader: false
        }
      },
      mounted () {
      this.init()
      this.getClientMobileDetail()
      this.getClientMobile()
    },
    created() {
      this.init()
      this.getClientMobileDetail()
      this.getClientMobile()
  
    },
    computed: {
      rows() {
        return this.clientMobileList.length
      }
    },
    methods: {
      init() {
        this.token = this.$auth.strategy.token.get()
      },
      getClientMobile() {
        this.loader = true
  
        this.$axios.get('clientMobile/allClientMobiles', {
          headers: {
            'Content-Type': 'application/json',
            'Access-Control-Allow-Origin': '*',
            'x-access-token': this.token},
        }, {withCredentials: true}).then((response) => {
          console.log(response);
          return this.clientMobileList = response.data
        }).catch((error) => {
          console.log(error)
        }).finally(() => {
          this.loader = false
        })
      },
      getClientMobileDetail() {
          this.$axios.get('clientMobile/clientMobileDetails', {
            headers: {
              'Content-Type': 'application/json',
              'Access-Control-Allow-Origin': '*',
              'x-access-token': this.token},
          }, {withCredentials: true}).then((response) => {
            console.log(response)
            // this.currentUser = response.data.data.email
            // if(response.data.data.role == 'Admin') {
            //   this.checkUser = true
            // } else {
            //   this.checkUser = false
            // }
            // console.log(this.currentUser)
          }).catch((error) => {
            console.log(error)
          })
        },
  
    }
  }
  </script>
  <style scoped>
  
  </style>
  