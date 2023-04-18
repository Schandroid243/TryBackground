<template>
  <div>
    <b-container class=" mt-4 col-md-12 col-xl-12 col-sm-12 p-0">
      <h5 class="text-danger"> <em>{{info}}</em> </h5>
      <b-row>
          <b-col v-if="loader" class="mt-4 mb-4">
          <b-row>
            <b-card class="col-md-12" style="height:100%">
              <b-skeleton animation="throb" width="85%"></b-skeleton>
              <b-skeleton animation="throb" width="55%"></b-skeleton>
              <b-skeleton animation="throb" width="70%"></b-skeleton>
            </b-card>
          </b-row>
        </b-col>

        <b-card v-else class="col-md-12 shadow p-0">
          <b-card-header class="d-flex bg-white p-1" style="justify-content:space-between">
            <h4>Profil</h4>
            <b-button @click="cancel" class="btn-info">
              <b-icon class="mx-2" icon="arrow-left"></b-icon>retour
            </b-button>
          </b-card-header>
          <b-card-body>
            <b-container class="p-0 col-md-12">
              <b-row>
                <b-col class="col-md-9 col-sm-12 col-xl-9">
                  <div class="">
                    <b-container class="mt-2 mb-2 p-0">
                      <label>Prénom :</label>
                      <em class="contact h5">{{contact.firstName}}</em>
                    </b-container>
                    <b-container class="mt-2 mb-2 p-0">
                      <label>Nom :</label>
                      <em class="h5">{{contact.name}}</em>
                    </b-container>
                    <b-container class="mt-2 mb-2 p-0">
                      <label>Post-Nom :</label>
                      <em class="h5">{{contact.lastName}}</em>
                    </b-container>
                    <b-container class="mt-2 mb-2 p-0">
                      <label>Organisation :</label>
                      <em class="h5">{{profile.organization}}</em>
                    </b-container>
                    <b-container class="mt-2 mb-2 p-0">
                      <label>Titre :</label>
                      <em class="h5">{{profile.title}}</em>
                    </b-container>
                    <b-container class="mt-2 mb-2 p-0">
                      <label>Tél. Bureau :</label>
                      <em class="h5">{{profile.phoneNumberWork}}</em>
                    </b-container>
                    <b-container class="mt-2 mb-2 p-0">
                      <label>Tél. Domicile :</label>
                      <em class="h5">{{profile.phoneNumberHome}}</em>
                    </b-container>
                    <b-container class="mt-2 mb-2 p-0">
                      <label>Email :</label>
                      <em class="h5">{{profile.email}}</em>
                    </b-container>
                    <b-container class="mt-2 mb-2 p-0">
                      <label>Website :</label>
                      <em class="h5">{{profile.website}}</em>
                    </b-container>
                    <b-container class="mt-2 mb-2 p-0">
                      <label>Adresse :</label>
                      <em class="h5">{{profile.adress}}</em>
                    </b-container>
                    <b-container class="mt-2 mb-2 p-0">
                      <label>Status :</label>
                      <em v-if="profile.status" class="h5">Validée</em>
                      <em v-else class="h5">Enregistrée</em>
                    </b-container>
                  </div>
                </b-col>
                <b-col class="col-md-3 col-sm-12 col-xl-3">
                  <div id="contact">
                    <vue-qr :text="vcard" :size="250">
                    </vue-qr>
                  </div>
                  <b-container>
                    <b-button @click="shot" class="btn-info" style="width:150px; height:auto">
                      <b-icon icon="download"></b-icon>
                    </b-button>
                  </b-container>
                </b-col>
                </b-row>
                <b-row>
              </b-row>
            </b-container>

          </b-card-body>
        </b-card>

      </b-row>

    </b-container>
  </div>
</template>

<script>
  import Vue from 'vue'
  import {
    BootstrapVueIcons
  } from 'bootstrap-vue'
  Vue.use(BootstrapVueIcons)
  import VueQr from 'vue-qr';
  import domtoimage from 'dom-to-image';
  import saveAs from 'file-saver';

  export default {
    components: {
      VueQr,
      domtoimage,
      saveAs
    },
    name: 'contactDetail',
    data() {
      return {
        id: "",
        contact: {
          firstName: '',
          name: '',
          lastName: '',
        },

        profile: {
          contact_id: '',
          title: '',
          organization: '',
          phoneNumberWork: '',
          phoneNumberHome: '',
          email: '',
          website: '',
          adress: '',
          status: '',
          token: ''
        },
        vcard: '',
        info: '',
        contactId: '',
        loader: false
      }
    },
    created() {
      this.init()
      this.getContact()
      this.getProfile()
    },
    methods: {
      init() {
        this.id = this.$route.params.id
        this.contactId = this.$route.params.contact_id
        this.token = this.$auth.strategy.token.get()
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
            this.profile = response.data
            this.vCard()
          }).catch((error) => {
            this.info = 'vous êtes hors-connexion'
          }).finally(() => {
            this.loader = false
          })
      },

       getContact() {
        this.$axios.get(`contact/contactDetail/${this.contactId}`, {
            headers: {
              'Content-Type': 'application/json',
              'Access-Control-Allow-Origin': '*',
              'x-access-token': this.token
            },
          }, {
            withCredentials: true
          }).then((response) => {
            this.contact = response.data

          }).catch((error) => {
            this.info = 'vous êtes hors-connexion'
          }).finally(() => {
            this.loader = false
          })
      },

       vCard() {
        console.log(this.profile.phoneNumberWork);
        this.vcard = "BEGIN:VCARD" + "\n" +
          "VERSION:4.0" + "\n" +
          "FN:" + this.contact.firstName + " " + this.contact.name + " " + this.contact.lastName + "\n" +
          "N;" + this.contact.name + "\n" +
          ";" + this.contact.lastName + "\n" +
          "ORG;CHARSET=UTF-8:" + this.profile.organization + "\n" +
          "TITLE;CHARSET=UTF-8:" + this.profile.title + "\n" +
          "ADR:;;" + this.profile.adress + ";;;;;" + "\n" +
          "TEL;TYPE=home;VALUE=uri:" + this.profile.phoneNumberHome + "\n" +
          "TEL;TYPE=work;VALUE=uri:" + this.profile.phoneNumberWork + "\n" +
          "EMAIL:" + this.profile.email + "\n" +
          "URL;CHARSET=UTF-8:" + this.profile.website + "\n" +
          "END:VCARD";
      },

      cancel() {
        this.$router.go(-1)
      },

      shot() {
        var domtoimage = require('dom-to-image');
        var fileName = 'Qr_code_contact_' + this.contact.firstName + '_' + this.contact.name + '.png'
        domtoimage.toBlob(document.getElementById('contact'))
          .then(function (blob) {
            window.saveAs(blob, fileName);
          });
      }
    }
  }

</script>

<style scoped>
  em {
    font-size: 12pt;
    padding-left: 10px;
  }

</style>
