<template>
  <section class="fill-height">
    <v-app-bar app class="elevation-1">
      <v-toolbar-title>Buscar</v-toolbar-title>
      <template v-slot:extension>
        <v-tabs align-with-title grow id="tabs-search">
          <v-tab exact to="/search/people">PERSONAS</v-tab>
          <v-tab exact to="/search/jobs">TRABAJOS</v-tab>
        </v-tabs>
      </template>
    </v-app-bar>
    <div class="section-search">
      <v-navigation-drawer permanent min-width="412px" width="412PX"></v-navigation-drawer>
      <v-container v-if="$route.query.q">
        <v-row>
          <v-col class="col-12 col-md-12">
            <search-bar label="Buscar personas...">
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
                  <v-list-item>
                    <v-list-item-title class="font-weight-light" @click="props.setSearchArea('name:')">Nombre:
                    </v-list-item-title>
                  </v-list-item>
                </v-list>
              </template>
            </search-bar>
          </v-col>
        </v-row>
      </v-container>
      <v-container fill-height align-center v-else>
        <v-row>
          <v-col class="col-md-2 col-12"></v-col>
          <v-col class="col-12 col-md-8">
            <search-bar label="Buscar personas..." :setSearchParams="setSearchParams">
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
                  <v-list-item>
                    <v-list-item-title class="font-weight-light" @click="props.setSearchArea('name:')">Nombre:
                    </v-list-item-title>
                  </v-list-item>
                </v-list>
              </template>
            </search-bar>
          </v-col>
          <v-col class="col-md-2 col-12"></v-col>
        </v-row>
      </v-container>
    </div>
  </section>
</template>

<script>
  import searchBar from '~/components/searchBar.vue'

  export default {
    components: {
      searchBar,
    },
    data() {
      return {
        optionsSearch: 0
      }
    },
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

  #tabs-search {
    margin-left: 412px;
  }

  .section-search {
    display: flex;
    height: 100%;
  }

</style>
