<template>
  <div class="fill-height">
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
      <v-navigation-drawer permanent fixed min-width="412px" width="412PX" class=section-sidebar>
        <sidebar-search-filters :aggregators="aggregators" :loadingAggregators="loadingAggregators">
        </sidebar-search-filters>
      </v-navigation-drawer>
      <v-container v-if="$route.query.q" class="section-main">
        <v-row>
          <v-col class="col-12 col-md-12">
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
        </v-row>
        <v-row class="justify-center">
          <v-col class="col-12 col-md-7" v-for="job in jobs.results" :key="job.id">
            <job-card :job="job"></job-card>
          </v-col>
        </v-row>
      </v-container>
      <v-container fill-height align-center v-else class="section-main">
        <v-row>
          <v-col class="col-md-2 col-12"></v-col>
          <v-col class="col-12 col-md-8">
            <search-bar label="Buscar trabajos...">
              <template slot-scope="props">
                <v-list>
                  <v-list-item>
                    <v-list-item-title class="font-weight-light" @click="props.setSearchArea('job/skill:')">Job/Skill:
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
          <v-col class="col-md-2 col-12"></v-col>
        </v-row>
      </v-container>
    </div>
  </div>
</template>

<script>
  import sidebarSearchFilters from '~/components/sidebarSearchFilters.vue'
  import searchBar from '~/components/searchBar.vue'
  import jobCard from '~/components/jobCard.vue'

  export default {
    components: {
      sidebarSearchFilters,
      searchBar,
      jobCard
    },
    data() {
      return {
        page: 1,
        jobs: {},
        aggregators: {
          organization: []
        },
        loadingAggregators: false,
      }
    },
    created() {
      this.findJobs()
      this.findAgreggators()
    },
    methods: {
      findJobs() {
        this.$axios.post(
          `https://search.torre.co/opportunities/_search/?offset=${this.offsetPage}&size=${this.sizePage}&page=${this.page}&aggregate=false`, {
            and: this.searchParams
          }).then((data) => {
          this.jobs = data.data
        })
      },
      findAgreggators() {
        this.loadingAggregators = true
        this.$axios.post(
          `https://search.torre.co/opportunities/_search/?offset=${this.offsetPage}&size=${this.sizePage}&page=${this.page}&aggregate=true`, {
            and: this.searchParams
          }).then((data) => {
          this.aggregators = data.data.aggregators
          this.loadingAggregators = false
        })
      },
    },
    computed: {
      offsetPage() {
        return (this.page - 1) * 12
      },
      sizePage() {
        return this.offsetPage + 12
      },
      searchParams() {
        let urlsParams = []
        if (this.$route.query.q) {
          let splitParams = this.$route.query.q.split(' AND ')
          urlsParams = splitParams.map((param) => {
            let splitParam = param.split(":")
            return {
              [splitParam[0]]: splitParam[1]
            }
          })
        }
        return urlsParams
      },
      '$route.query.q': function (val) {
        this.findJobs()
      },
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


  .section-sidebar {
    margin-top: 112px !important;
    height: calc(100% - 112px) !important;
  }

  .section-main {
    margin-left: 412px;
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
