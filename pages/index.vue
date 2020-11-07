<template>
  <v-container fluid fill-height class="d-flex align-center">
    <v-app-bar fixed app class="d-flex justify-space-around" width="100%">
      <template v-slot:default>
        <v-tabs align-with-title grow v-model="optionsSearch" background-color="transparent">
          <v-tab>Trabajos</v-tab>
        </v-tabs>

      </template>
    </v-app-bar>
    <v-tabs-items v-model="optionsSearch" id="tabs">
      <v-tab-item>
        <v-row>
          <v-col class="col-md-12 col-12 d-flex justify-center">
            <img src="/torre.png">
          </v-col>
          <v-col class="col-md-3 col-12"></v-col>
          <v-col class="col-md-6 col-12">
            <search-bar label="Buscar trabajos...">
              <template slot-scope="props">
                <v-list>
                  <v-list-item>
                    <v-list-item-title class="font-weight-light" @click="props.setSearchArea('Job/Skill:')">Job/Skill:
                    </v-list-item-title>
                  </v-list-item>
                  <v-list-item>
                    <v-list-item-title class="font-weight-light" @click="props.setSearchArea('organization:')">
                      Organization:
                    </v-list-item-title>
                  </v-list-item>
                </v-list>
              </template>
            </search-bar>
          </v-col>
          <v-col class="col-md-3 col-12"></v-col>
        </v-row>

      </v-tab-item>
    </v-tabs-items>

  </v-container>
</template>

<script>
  import Logo from '~/components/Logo.vue'
  import searchBar from '~/components/searchBar.vue'

  export default {
    components: {
      searchBar,
    },
    data() {
      return {
        optionsSearch: 0
      }
    }
  }

</script>

<style scoped>
  .center-img {
    margin: 0 auto;
  }

  #tabs {
    width: 100%;
    background: transparent;
  }

</style>
<style>
  .v-toolbar__content {
    width: 90%;
  }

</style>
