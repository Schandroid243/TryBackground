<template>

</template>

<script>
export default {
  data() {

  },

  created() {
    this.redirect();
  },

  methods: {
    redirect() {
      this.$router.push({name: 'login'})
    }
  }
}
</script>

<style>

</style>
