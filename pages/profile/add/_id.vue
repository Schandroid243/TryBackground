<template>
  <b-container fluid>
    <b-row class="myIndex justify-content-center">
      <b-container fluid v-if="loader" class="mt-4 mb-4">
            <b-row>
              <b-card class="col-md-12" height="100">
                <b-skeleton animation="throb" width="85%"></b-skeleton>
                <b-skeleton animation="throb" width="55%"></b-skeleton>
                <b-skeleton animation="throb" width="70%"></b-skeleton>
              </b-card>
            </b-row>
      </b-container>
      <b-container fluid v-else class="col-md-9 justify-content-center">
      <b-card class="shadow" bg-variant="white">
        <b-row class="mt-1 mb-1">
              <b-col lg="3">
                <h6>Contact</h6>
              </b-col>
              <b-col lg="9">
                <b-form-group>
                  <b-form-input disabled v-model="contact.firstName" class="mt-1 mb-1" id="FirstName"
                    placeholder="Entrer votre prénom"></b-form-input>
                  <b-form-input disabled v-model="contact.name" class="mt-1 mb-1" id="FirstName" placeholder="Entrer votre nom">
                  </b-form-input>
                  <b-form-input disabled v-model="contact.lastName" class="mt-1 mb-1" id="LastName"
                    placeholder="Entrez votre PostNom"></b-form-input>
                </b-form-group>
              </b-col>
            </b-row>
        <b-row class="mt-1 mb-1">
          <b-col lg="3">
            <h6>Organisation</h6>
          </b-col>
          <b-col lg="9">
            <b-form-group>
              <b-form-input v-model="form.organization" class="mt-1 mb-1" id="Enterprise"
                placeholder="Nom de l'entreprise"></b-form-input>
              <b-form-input v-model="form.title" class="mt-1 mb-1" placeholder="Votre Titre"></b-form-input>
            </b-form-group>

          </b-col>
        </b-row>
        <b-row class="mt-1 mb-1">
          <b-col lg="3">
            <h6>Téléphone</h6>
          </b-col>
          <b-col lg="9">
            <b-form-group>
              <b-form-input v-model="form.phoneNumberWork" class="mt-1 mb-1" id="Work" placeholder="Tél. Bureau">
              </b-form-input>
              <b-form-input v-model="form.phoneNumberHome" class="mt-1 mb-1" id="Work" placeholder="Tél. Domicile">
              </b-form-input>
            </b-form-group>

          </b-col>
        </b-row>
        <b-row class="mt-1 mb-1">
          <b-col lg="3">
            <h6>Email</h6>
          </b-col>
          <b-col lg="9">
            <b-form-group>
              <b-form-input v-model="form.email" class="mt-1 mb-1" id="Email" placeholder="Adresse email">
              </b-form-input>
            </b-form-group>

          </b-col>
        </b-row>
        <b-row class="mt-1 mb-1">
          <b-col lg="3">
            <h6>Website</h6>
          </b-col>
          <b-col lg="9">
            <b-form-group>
              <b-form-input v-model="form.website" class="mt-1 mb-1" id="Website" placeholder="Votre Website">
              </b-form-input>
            </b-form-group>

          </b-col>
        </b-row>
        <b-row class="mt-1 mb-1">
          <b-col lg="3">
            <h6>Adresse</h6>
          </b-col>
          <b-col lg="9">
            <b-form-group>
              <b-form-input v-model="form.adress" class="mt-1 mb-1" id="Address" placeholder="Votre adresse">
              </b-form-input>
            </b-form-group>
          </b-col>
        </b-row>
      </b-card>
    </b-container>
    </b-row>
    <b-container class=" mt-4 mb-4 col-md-9">
      <b-button-group class="d-flex align-items-center justify-content-sm-end">
        <b-button @click="submitForm" class="btn-info text-white col-md-4 mx-2 shadow">
          Enregistrer
        </b-button>
        <b-button @click="cancel" class="btn-danger text-white col-md-4 mx-2 shadow">Annuler</b-button>
      </b-button-group>
    </b-container>
  </b-container>
</template>

<script>
  export default {
    name: 'ContactCard',
    data() {
      return {
        id: '',
        contact: {
          firstName: '',
          name: '',
          lastName: ''
        },
        form: {
          contact_id: 0,
          organization: '',
          phoneNumberWork: '',
          phoneNumberHome: '',
          title: '',
          email: '',
          website: '',
          adress: ''

        },
        loader: '',
        contacts:[],
      }
    },
    mounted() {
      this.init()
      this.getContact()
    },
    methods: {
      init() {
        this.token = this.$auth.strategy.token.get()
        this.id = this.$route.params.id
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
      submitForm() {
          this.form.contact_id = this.id
          this.$axios.post('profile/addProfile', this.form, {
          headers: {
            'Content-Type': 'application/json',
            'Access-Control-Allow-Origin': '*',
            'x-access-token': this.token
          },
        }, {
          withCredentials: true
        }).then((response) => {

          this.form = response.data
          console.log(this.form)
        }).catch((error) => {
          console.log(error)
        })
        this.$router.go(-1)


      },
      cancel() {
        this.$router.go(-1)
      }
    }
  }

</script>
