<template>
  <div class="fill-height">
    <v-app-bar app class="elevation-1">
      <v-toolbar-title>Buscar</v-toolbar-title>
      <template v-slot:extension>
        <v-tabs grow id="tabs-search">
          <v-tab exact to="/search/jobs">TRABAJOS</v-tab>
        </v-tabs>
      </template>
    </v-app-bar>
    <div class="section-search">
      <v-navigation-drawer permanent fixed min-width="412px" width="412PX" class="section-sidebar hidden-sm-and-down">
        <search-filters :aggregators="aggregators" :loadingAggregators="loadingAggregators">
        </search-filters>
      </v-navigation-drawer>
      <v-container v-if="$route.query.q" class="section-main">
        <v-row>
          <v-col class="col-10 col-md-12">
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
          <v-col class="col-2 hidden-md-and-up">
            <v-btn icon class="mt-2" @click="openModalFilters = true">
              <v-icon color="primary"> mdi-filter</v-icon>
            </v-btn>
          </v-col>
        </v-row>
        <v-row class="justify-center" v-if="mdAndUp">
          <v-col class="col-12 col-md-9 col-lg-7" v-for="job in jobs.results" :key="job.id">
            <job-card :job="job"></job-card>
          </v-col>
        </v-row>
        <v-row class="justify-center" v-else>
          <v-col class="col-12 col-md-7" v-for="job in jobs.results" :key="job.id">
            <job-card-mobile :job="job"></job-card-mobile>
          </v-col>
        </v-row>
        <v-row>
          <v-col class="col-12 col-md-12">
            <v-pagination v-model="page" :length="Math.ceil(jobs.total/12)" :total-visible="10"></v-pagination>
          </v-col>
        </v-row>
      </v-container>
      <v-container fill-height align-center v-else class="section-main">
        <v-row>
          <v-col class="col-md-12 col-12 d-flex justify-center">
            <img src="/torre.png" width="120">
          </v-col>
          <v-col class="col-md-2 col-12"></v-col>
          <v-col class="col-10 col-md-8">
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
          <v-col class="col-md-2 col-2 hidden-md-and-up">
              <v-btn icon @click="openModalFilters = true">
                <v-icon color="primary"> mdi-filter</v-icon>
              </v-btn>
          </v-col>
        </v-row>
      </v-container>
    </div>
    <v-dialog fullscreen v-model="openModalFilters">
      <search-filters @closeFilters="openModalFilters = false" :aggregators="aggregators"
        :loadingAggregators="loadingAggregators">
      </search-filters>
    </v-dialog>
  </div>
</template>

<script>
  import searchFilters from '~/components/searchFilters.vue'
  import searchBar from '~/components/searchBar.vue'
  import jobCard from '~/components/jobCard.vue'
  import jobCardMobile from '~/components/jobCardMobile.vue'

  export default {
    components: {
      searchFilters,
      searchBar,
      jobCard,
      jobCardMobile
    },
    data() {
      return {
        page: 1,
        jobs: {},
        aggregators: {
          organization: []
        },
        openModalFilters: false,
        loadingAggregators: false,
        loadingJobs: false,
      }
    },
    created() {
      this.findJobs()
      this.findAgreggators()
    },
    methods: {
      findJobs() {
        this.jobs = {}
        this.loadingJobs = true
        this.$axios.post(
          `https://search.torre.co/opportunities/_search/?offset=${this.offsetPage}&size=${this.sizePage}&page=${this.page}&aggregate=false`, {
            and: this.searchParams
          }).then((data) => {
          this.jobs = data.data
          this.loadingJobs = false
        }).catch((err) => {
          this.loadingJobs = false
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
        //Split and set search params using q params
        let urlsParams = []
        if (this.$route.query.q) {
          let splitParams = this.$route.query.q.split(' AND ')
          urlsParams = splitParams.map((param) => {
            let returnValue = {}
            let splitParam = param.split(":")
            if(splitParam[0]=='compensationrange') {
              let splitValue = splitParam[1].split(" ")
              let splitPriceAndTime = splitValue[1].split("/")
              let splitAmmount = splitPriceAndTime[0].split("-")
              returnValue = {'currency':splitValue[0],'minAmmount':splitAmmount[0],'maxAmmount':splitAmmount[1],'periodicity':splitPriceAndTime[1]}
            }else if(splitParam[0]=='status' || splitParam[0]=='type') {
              returnValue = {'code':splitParam[1]}  
            }else if(splitParam[0]=='organization') {
              returnValue = {'term':splitParam[1]}  
            }
            return {[splitParam[0]]:returnValue}
          })
        }
        return urlsParams
      },
      mdAndUp() {
        return this.$vuetify.breakpoint.mdAndUp
      }
    },
    watch: {
      page() {
        this.findJobs()
      },
      '$route.query.q': function (val) {
        //Finding jobs watching changes in q param in url
        this.findJobs()
        this.findAgreggators()
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

  @media(min-width:1024px) {
    .section-sidebar {
      margin-top: 112px !important;
      height: calc(100% - 112px) !important;
    }

    .section-main {
      margin-left: 412px;
      width: calc(100% - 412px);
    }

  }

</style>
<style>
  .v-toolbar__content {
    width: 90%;
  }

  #tabs-search {
    margin-left: 412px;
    width: calc(100% - 412px);
  }

  .section-search {
    display: flex;
    height: 100%;
  }

</style>
