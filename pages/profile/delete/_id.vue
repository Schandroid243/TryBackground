<template>
  <b-container >
    <b-modal v-model="modalShow"
            hide-footer
            class="justify-content-center text-center"
            no-close-on-backdrop>
      <template #modal-header>
        <b-col>
          <b-row class="d-block text-center">
            <h5>Voulez-vous supprimer le profil ?</h5>
        </b-row>
        </b-col>
      </template>
        <b-row>
          <b-container class="d-flex justify-content-center col-md-6"
    style="gap:20px">
    <b-button class="mt-3 btn-info" block @click="cancel">Annuler</b-button>
    <b-button class="mt-3 btn-danger" block @click="deleteProfile">Supprimer</b-button>
    </b-container>
        </b-row>


    </b-modal>
  </b-container>
</template>

<script>
export default {
data() {
    return {

        token: '',
    }
},
created() {
    this.init()
    this.modalShow()

},
methods: {
    modalShow() {
        this.modalShow = true
    },
    init() {
          this.token = this.$auth.strategy.token.get()
          this.id = this.$route.params.id
        },
        deleteProfile() {
        this.$axios.delete(`profile/deleteProfile/${this.id}`, {
          headers: {
            'Content-Type': 'application/json',
            'Access-Control-Allow-Origin': '*',
            'x-access-token': this.token
          },
        }, {
          withCredentials: true
        }).then((response) => {
          this.$router.go(-1)
        })
      },
        cancel() {
          this.$router.go(-1)
        }
}
}
</script>

<style>
