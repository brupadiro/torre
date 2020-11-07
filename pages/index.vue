<template>
  <v-container fluid fill-height class="d-flex align-center">
    <v-app-bar fixed app class="d-flex justify-space-around" width="100%">
      <template v-slot:default>
        <v-tabs align-with-title grow>
          <v-tab>Personas</v-tab>
          <v-tab>Trabajos</v-tab>
        </v-tabs>

      </template>
    </v-app-bar>
    <v-row>
      <v-col class="col-md-12 col-12 d-flex justify-center">
        <img src="/torre.png">
      </v-col>
      <v-col class="col-md-3 col-12"></v-col>
      <v-col class="col-md-6 col-12">
        <searchBarJobs></searchBarJobs>
      </v-col>
      <v-col class="col-md-3 col-12"></v-col>
    </v-row>
  </v-container>
</template>

<script>
  import Logo from '~/components/Logo.vue'
  import searchBarJobs from '~/components/searchBarJobs.vue'

  export default {
    components: {
      Logo,
      searchBarJobs
    }
  }

</script>

<style scoped>
  .center-img {
    margin: 0 auto;
  }

  .v-toolbar__content {
    width: 90%;
  }
</style>
