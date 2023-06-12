<template>
  <div class="mt-4" style="padding-top: 15px">
          <b-container class="mt-4 mb-4 text-white">
            <h3>Editer le profil du contact</h3>
          </b-container>
    <b-container fluid v-if="loader" class="mt-4 mb-4">
      <b-row class="align-items-center justify-content-center">
        <b-card class="col-md-9 col-xl-9" height="100">
          <b-skeleton animation="throb" width="85%"></b-skeleton>
          <b-skeleton animation="throb" width="55%"></b-skeleton>
          <b-skeleton animation="throb" width="70%"></b-skeleton>
        </b-card>
      </b-row>
    </b-container>
    <b-container fluid v-else>
      <b-row class="align-items-center justify-content-center">
        <div class=" col-md-12 col-xl-9 col-lg-12 text-dark">
          <b-card bg-variant="white" class="col-md-12 shadow">
            <b-row class="mt-2 mb-2">
              <h5 class="text-danger"> <em>{{info}}</em> </h5>
            </b-row>
            <b-row class="mt-2 mb-2">
              <b-col lg="3">
                <label for="name" class="">Organisation</label>
              </b-col>
              <b-col lg="9">
                <b-form-group>
                  <b-form-input v-model="form.organization" class="mt-2 mb-2" id="Enterprise"
                    placeholder="Nom de l'entreprise"></b-form-input>
                  <b-form-input v-model="form.title" class="mt-2 mb-2" placeholder="Votre Titre"></b-form-input>
                </b-form-group>

              </b-col>
            </b-row>

            <b-row class="mt-2 mb-2">

              <b-col lg="3">
                <label for="name" class="">Téléphone</label>
            
                
                
              </b-col>
              
              <b-col lg="9">
                <b-form-group>
                  <label for="name" class="">Bureau</label>
                  <b-form-input v-model="form.phoneNumberWork" class="mt-2 mb-2" id="Work" placeholder="Tél. Bureau">
                  </b-form-input>
                  <label for="name" class="">Domicile</label>
                  <b-form-input v-model="form.phoneNumberHome" class="mt-2 mb-2" id="Work" placeholder="Tél. Domicile">
                  </b-form-input>
                </b-form-group>

              </b-col>
            </b-row>
            <b-row class="mt-2 mb-2">
              <b-col lg="3">
                <label for="name" class="">Email</label>
              </b-col>
              <b-col lg="9">
                <b-form-group>
                  <b-form-input v-model="form.email" class="mt-2 mb-2" id="Email" placeholder="Adresse email">
                  </b-form-input>
                </b-form-group>

              </b-col>
            </b-row>
            <b-row class="mt-2 mb-2">
              <b-col lg="3">
                <label for="name" class="">Website</label>
              </b-col>
              <b-col lg="9">
                <b-form-group>
                  <b-form-input v-model="form.website" class="mt-2 mb-2" id="Website" placeholder="Votre Website">
                  </b-form-input>
                </b-form-group>

              </b-col>
            </b-row>
            <b-row class="mt-2 mb-2">
              <b-col lg="3">
                <label for="name" class="">Adresse</label>
              </b-col>
              <b-col lg="9">
                <b-form-group>
                  <b-form-input v-model="form.adress" class="mt-2 mb-2" id="Address" placeholder="Votre adresse">
                  </b-form-input>
                </b-form-group>
              </b-col>
            </b-row>
            <b-row class="mt-1 mb-1" style="">
              <template>
                <b-col lg="3">
                  <h6>Status</h6>
                </b-col>
                <b-col lg="9" >
                  <b-form-group v-if="checkUserRole" v-slot="{ ariaDescribedby }" >
                    <b-form-radio v-model="form.status" :aria-describedby="ariaDescribedby" name="some-radios" value="false">Enregistrée</b-form-radio>
                    <b-form-radio v-model="form.status" :aria-describedby="ariaDescribedby" name="some-radios" value="true">Validée</b-form-radio>
                  </b-form-group>
                  <b-form-group v-else v-slot="{ ariaDescribedby }">
                    <b-form-radio disabled v-model="form.status" :aria-describedby="ariaDescribedby" name="some-radios" value="false">Enregistrée</b-form-radio>
                    <b-form-radio disabled v-model="form.status" :aria-describedby="ariaDescribedby" name="some-radios" value="true">Validée</b-form-radio>
                  </b-form-group>
                </b-col>
                  <!-- <div class="mt-3">Selected: <strong>{{ selected }}</strong></div> -->
              </template>
            </b-row>
          </b-card>
          <b-container class="d-flex mt-4 mb-4  d-flex align-items-center justify-content-md-end">
            <b-button @click="submitForm" class="btn-secondary text-white col-md-4 mx-2">
              <b-icon icon="save-fill" width="22px" height="22px"></b-icon>
              Enregistrer
            </b-button>
            <b-button @click="cancel" class="btn-danger text-white col-md-4 mx-2">Annuler</b-button>
          </b-container>
        </div>
      </b-row>
    </b-container>
  </div>
</template>

<script>
  export default {
    name: 'EditerContact',
    data() {
      return {
        id: "",
        checkUserRole: false,
        currentUser:{
          id: 0,
          name: '',
          email: '',
          role: ''
        },
        form: {
          title: '',
          organization: '',
          phoneNumberWork: '',
          phoneNumberHome: '',
          email: '',
          website: '',
          adress: '',
          status: ''

        },
        token: '',
        info: '',
        loader: false

      }
    },
    created() {
      this.init()
      this.getUsers()
      this.getProfile()
    },
    methods: {
      init() {
        this.id = this.$route.params.id
        this.token = this.$auth.strategy.token.get()
      },
      getUsers() {
            this.$axios.get('auth/userDetails', {
              headers: {
                'Content-Type': 'application/json',
                'Access-Control-Allow-Origin': '*',
                'x-access-token': this.token},
            }, {withCredentials: true}).then((response) => {
              if(response.data.data.role == 'Admin') {
                this.checkUserRole = true;
              } else {
                this.checkUserRole = false;
              }
              return this.currentUser = response.data.data
            }).catch((error) => {
            })
          },
      getProfile() {
        this.loader = true
        this.$axios.get(`profile/profileDetail/${this.id}`, {
          headers: {
            'Content-Type': 'application/json',
            'Access-Control-Allow-Origin': '*',
            'x-access-token': this.token
          },
        }, {
          withCredentials: true
        }).then((response) => {
          this.form = response.data
        }).catch((error) => {
          this.info = 'vous êtes hors-connexion'
        }).finally(() => {
          this.loader = false
        })
      },
      cancel() {
        this.$router.go(-1)
      },
      submitForm() {
        this.$axios.put(`profile/updateProfile/${this.id}`, this.form, {
          headers: {
            'Content-Type': 'application/json',
            'Access-Control-Allow-Origin': '*',
            'x-access-token': this.token
          },
        }, {
          withCredentials: true
        }).then((response) => {
          
        }).catch((error) => {

        })
        this.$router.go(-1)
      },
    }

  }

</script>

<style scoped>
.align {
}
</style>
