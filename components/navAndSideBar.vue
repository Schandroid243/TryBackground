<template>
  <div>
    <b-container fluid>
      <b-row>
        <b-navbar toggleable="lg" class="background shadow fixed-top">
          <b-navbar-brand class="px-2">
            <b-container class="d-flex" style="gap:20px">
              <nuxt-link class="text-decoration-none d-flex px-0" to="/home">
                <b-img :src="require('@/assets/DaurecardAPP.png')" width="55" height="40"></b-img>
                <h3 class="text-white mx-2">DaureCard</h3>
                <div class="border-right mx-4"></div>
              </nuxt-link>
              <b-button class="btn mx-4" @click="checkSideBar()">
                <b-icon icon="menu-app"></b-icon>
              </b-button>
            </b-container>

          </b-navbar-brand>
          <b-navbar-toggle target="nav-collapse"></b-navbar-toggle>
            <b-collapse id="nav-collapse" is-nav>
              <b-navbar-nav class="ml-auto align-items-center justify-content-center">
                <b-nav-item-dropdown right class="px-4">
                  <template #button-content>
                    <img src="~/assets/profile.jpeg" height="30px" width="30px" style="border-radius:100%">
                  </template>
                  <b-dropdown-item>
                    <em class="text-dark mx-2">{{currentUser.name}}</em>
                  </b-dropdown-item>
                  <b-dropdown-item @click="logOut">
                    Deconnexion
                  </b-dropdown-item>

                </b-nav-item-dropdown>
              </b-navbar-nav>
            </b-collapse>
        </b-navbar>
      </b-row>
      <b-row class="flex no-wrap">
        <template>
  <div class="flex-column mb-sm-auto align-items-center align-items-sm-start">
    <div id="mySidenav" class="sidenav">
      <div style="padding-top:40px"></div>
          <b-nav pills class="">
            <b-nav-item  class=" mt-2 mb-2">
                    <nuxt-link to="/home" class=" d-flex text-decoration-none text-white">
                      <b-icon icon="grid-fill" variant="white" width="22px" height="22px"></b-icon>
                      <h6 class="px-3 text-white">Bureau</h6>
                    </nuxt-link>
                  </b-nav-item>
                  <b-nav-item @click="clientMobile" class="mt-2 mb-2 ">
                    <nuxt-link to="clientMobileList" class="d-flex text-decoration-none text-white">
                      <b-icon icon="person-badge-fill" variant="white" width="22px" height="22px"></b-icon>
                      <h6 class="px-3 text-white">Clients</h6>
                    </nuxt-link>
                  </b-nav-item>
                  <b-nav-item @click="contact" class=" mt-2 mb-2 ">
                    <nuxt-link to="contactList" class="d-flex text-decoration-none text-white">
                      <b-icon icon="people-fill" variant="white" width="22px" height="22px"></b-icon>
                      <h6 class="px-3 text-white">Contact</h6>
                    </nuxt-link>
                  </b-nav-item>
                  
                  <b-nav-item @click="utilisateur" class="mt-2 mb-2 ">
                    <nuxt-link to="userList" class="d-flex text-decoration-none text-white">
                      <b-icon icon="bag-fill" variant="white" width="22px" height="22px"></b-icon>
                      <h6 class="px-3 text-white">Utilisateur</h6>
                    </nuxt-link>
                  </b-nav-item>
          </b-nav>
        </div>
  </div>
        </template>

          <b-col class="col-md-12 mt-4 p-3 background min-vh-100">
            <div id="main">
          <nuxt />
        </div>
        </b-col>
      </b-row>
    </b-container>
  </div>
</template>
<script lang="js">

  import Vue from 'vue'
  import {
    BootstrapVue, BootstrapVueIcons,
   
  } from 'bootstrap-vue'
  Vue.use(BootstrapVue)
  Vue.use(BootstrapVueIcons)

  export default {

    components: {

    },

    data() {
      return {
        token: '',
        currentUser: {
          name: '',
          role: '',
          id: ''
        },
        refreshData: {
          email: '',
        },
        isOpen: false
      }

    },
    created() {
      this.init()
      this.getUsers()
      window.onpopstate = event => {
      if (
        window.localStorage.getItem("info") !== null &&
        this.$route.path == "/"
      ) {
        this.$router.push({name: 'home'});
      }

      if (
        window.localStorage.getItem("info") !== null &&
        this.$route.path == "/home"
      ) {
        this.$router.push({name: 'home'});
      }
    };
    },

    methods: {
    checkSideBar() {
      if(this.isOpen) {
        this.closeNav()
      } else {
        this.openNav()
      }
    },
    openNav() {
    this.isOpen = true;
    var mySidenav = document.getElementById("mySidenav");
    var main = document.getElementById("main");
      if (window.screen.width <= 450 ) {
        mySidenav.style.width = "250px";
        mySidenav.style.zIndex = 1000;
        main.style.marginLeft = "-250px";
      } else {
        mySidenav.style.width = "250px";
        main.style.marginLeft = "250px";
      }
      },
      closeNav() {
          this.isOpen = false
        document.getElementById("mySidenav").style.width = "0";
        document.getElementById("main").style.marginLeft = "0";
      },

      init() {
        this.token = this.$auth.strategy.token.get()
      },
      contact() {
        this.$router.push({name:'contact-listContact'})
      },
      home() {
        this.$router.push({name:'home'})
      },
      utilisateur() {
        this.$router.push({name:'user-listUser'})
      },
      clientMobile() {
        this.$router.push({name:'clientMobile-listClientMobile'})
      },
      getUsers() {
            this.$axios.get('auth/userDetails', {
              headers: {
                'Content-Type': 'application/json',
                'Access-Control-Allow-Origin': '*',
                'x-access-token': this.token},
            }, {withCredentials: true}).then((response) => {
              return this.currentUser = response.data.data
            }).catch((error) => {
              console.log(error)
            })
          },
      logOut() {
        this.$auth.logout()
        this.$router.replace({
          path: '/login'
        })
      },

    }
  }

</script>
<style>

</style>
