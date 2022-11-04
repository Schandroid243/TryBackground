<template>
  <div>
    <b-container class=" mt-4 col-md-12 col-xl-12 col-sm-8">
      <h5 class="text-danger"> <em>{{info}}</em> </h5>
      <b-row>
        <b-container>
          <b-col v-if="loader" class="mt-4 mb-4">
          <b-row>
            <b-card class="col-md-12" style="height:100%">
              <b-skeleton animation="throb" width="85%"></b-skeleton>
              <b-skeleton animation="throb" width="55%"></b-skeleton>
              <b-skeleton animation="throb" width="70%"></b-skeleton>
            </b-card>
          </b-row>
        </b-col>

        <b-card v-else class="col-md-12 shadow">
          <b-card-header class="d-flex bg-white" style="justify-content:space-between">
            <h4>Informations du profil</h4>
            <b-button @click="cancel" class="btn-info">
              <b-icon class="mx-2" icon="arrow-left"></b-icon>retour
            </b-button>
          </b-card-header>
          <b-card-body>
            <b-row class="justify-content-center">
              <b-col cols="8" class="col-md-8 col-sm-6 col-xl-8">
                <div class="">
                  <b-container class="mt-2 mb-2">
                    <label>Prénom :</label>
                    <em class="contact h5">{{contact.firstName}}</em>
                  </b-container>
                  <b-container class="mt-2 mb-2">
                    <label>Nom :</label>
                    <em class="h5">{{contact.name}}</em>
                  </b-container>
                  <b-container class="mt-2 mb-2">
                    <label>Post-Nom :</label>
                    <em class="h5">{{contact.lastName}}</em>
                  </b-container>
                  <b-container class="mt-2 mb-2">
                    <label>Organisation :</label>
                    <em class="h5">{{profile.organization}}</em>
                  </b-container>
                  <b-container class="mt-2 mb-2">
                    <label>Titre :</label>
                    <em class="h5">{{profile.title}}</em>
                  </b-container>
                  <b-container class="mt-2 mb-2">
                    <label>Tél. Bureau :</label>
                    <em class="h5">{{profile.phoneNumberWork}}</em>
                  </b-container>
                  <b-container class="mt-2 mb-2">
                    <label>Tél. Domicile :</label>
                    <em class="h5">{{profile.phoneNumberHome}}</em>
                  </b-container>
                  <b-container class="mt-2 mb-2">
                    <label>Email :</label>
                    <em class="h5">{{profile.email}}</em>
                  </b-container>
                  <b-container class="mt-2 mb-2">
                    <label>Website :</label>
                    <em class="h5">{{profile.website}}</em>
                  </b-container>
                  <b-container class="mt-2 mb-2">
                    <label>Adresse :</label>
                    <em class="h5">{{profile.adress}}</em>
                  </b-container>
                </div>
              </b-col>
              <b-col cols="4" class="col-md-4 col-sm-4 col-xl-4">
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
          </b-card-body>
        </b-card>
        </b-container>

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
      this.getProfile()
      this.getContact()

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
            console.log(response)
            this.profile = response.data
          }).catch((error) => {
            console.log(error)
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
            console.log(response)
            this.contact = response.data
            console.log('name is :' + this.contact.name)
            this.vCard()

          }).catch((error) => {
            console.log(error)
            this.info = 'vous êtes hors-connexion'
          }).finally(() => {
            this.loader = false
          })
      },

      vCard() {
        this.vcard = "BEGIN:VCARD" + "\n" +
          "VERSION:3.0" + "\n" +
          "FN:" + this.contact.firstName + " " + this.contact.name + " " + this.contact.lastName + "\n" +
          "N;" + this.contact.name + "\n" +
          ";" + this.contact.lastName + "\n" +
          "ORG;CHARSET=UTF-8:" + this.profile.organization + "\n" +
          "TITLE;CHARSET=UTF-8:" + this.profile.title + "\n" +
          "ADR:;;" + this.profile.adress + ";;;;;" + "\n" +
          "TEL;TYPE=home;VALUE=uri:tel" + this.profile.phoneNumberWork + "\n" +
          "TEL;TYPE=work;VALUE=uri:tel" + this.profile.phoneNumberHome + "\n" +
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
