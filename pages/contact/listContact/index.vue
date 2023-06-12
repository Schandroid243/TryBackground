<template>
  <div class=" mt-4">
      <b-row class="align-items-center justify-content-center">
        <div class=" col-md-12 col-xl-12 col-lg-12 text-dark">
          <b-container class="mt-4 mb-4 text-danger h5">
            <h5 class="text-danger"> <em>{{info}}</em> </h5>
          </b-container>
          <div class="d-flex"
               style="justify-content:space-between;
                padding-top: 15px;">
            <h4 class="text-white">Liste des contacts</h4>
            <!-- <nuxt-link class="text-decoration-none" :to="{name: 'contact-add'}">
              <b-button class="btn btn-secondary shadow">
                <b-icon icon="person-plus-fill" width="22px" height="22px"></b-icon>
                <span class="px-2">Créer</span>
              </b-button>
            </nuxt-link> -->
          </div>
          <b-container fluid class="mt-4 mb-4 col-md-12 col-xl-12 col-sm-12">
            <b-row>
              <form class="justify-content-center mt-4 mb-4">
                <input size="md" class="form-control" type="search" v-model="filter"
                  placeholder="Recherchez contact...">
              </form>
              <template>
                <b-container fluid v-if="loader" class="mt-4 mb-4">
                  <b-row>
                  <b-card class="col-md-12" height="100">
                    <b-skeleton animation="throb" width="85%"></b-skeleton>
                    <b-skeleton animation="throb" width="55%"></b-skeleton>
                    <b-skeleton animation="throb" width="70%"></b-skeleton>
                  </b-card>
                  </b-row>
                </b-container>
                <b-table v-else class="table-light shadow text-left" striped hover id="table" :items="listContact" :fields="fields"
                  :filter="filter" :per-page="perPage" :current-page="currentPage" small>
                  <col  v-for="(contact, i) in listContact" :key="i">
                  <template #cell(first_name)="contact"> {{contact.item.firstName}} </template>
                  <template #cell(last_name)="contact"> {{contact.item.lastName}} </template>
                  <template #cell(status)="contact">
                    <p class="text-danger w-300" v-if="contact.item.paymentStatus == false"> desactivate </p>
                    <p class="text-info w-800" v-else> activate </p>
                  </template>

                  <template #cell(actions)="contact">
                    <b-container class="justify-content-left align-items-center p-0" style="gap:5px">
                      <nuxt-link class="btn btn-secondary text-decoration-none text-white"
                      :to=" {name: 'contact-edit-id', params: {id: contact.item.id}} ">
                      <b-icon icon="pencil-square"></b-icon>
                    </nuxt-link>
                    <nuxt-link class="btn btn-info text-decoration-none text-white"
                      :to=" {name: 'contact-show-id', params: {id: contact.item.id}}">
                      <b-icon icon="file-earmark-person"></b-icon>
                    </nuxt-link>
                    <nuxt-link class="btn btn-danger text-decoration-none text-white"
                      :to=" {name: 'contact-delete-id', params: {id: contact.item.id}}">
                      <b-icon icon="eraser-fill"></b-icon>
                    </nuxt-link>
                    </b-container>

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
  export default {

    data() {
      return {
        perPage: 10,
        currentPage: 1,

        filter: '',
        fields: ['  ', { key: 'first_name', sortable: true}, 'name', 'last_name','status', 'actions'],
        listContact: [],
        listProfile: [],

        dataProfile: {
          id: '',
          contact_id: '',
          organization: '',
        },
        
        token: '',
        currentUser: '',
        info:'',
        loader: false
      }
    },

    computed: {
      rows() {
        return this.listContact.length
      }
    },

    created() {
      this.init()
      this.getContacts()
    },

    mounted() {
      this.init()
      this.getContacts()
    },
    methods: {

      init() {
        this.token = this.$auth.strategy.token.get()
        if(this.token == '') {
          this.$auth.refreshTokens();
        }
      },
       async getContacts() {
        this.loader = true
        try {

          let response = await this.$axios.get('contact/allContacts', {
          headers: {
            'Content-Type': 'application/json',
            'Access-Control-Allow-Origin': '*',
            'x-access-token': this.token
          },
        }, {
            withCredentials: true
        })
        return this.listContact = response.data
        } catch (err) {
          this.info = 'Vous êtes hors-conexion'
        } finally {
          this.loader = false

        }
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
            if(element.contact_id == this.contact.item.id) {
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
    }
  }

</script>
<style scoped>

</style>
