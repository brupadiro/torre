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
      <v-navigation-drawer permanent min-width="412px" width="412PX">
        <h2 class="search-filters-title mt-2">Buscar trabajos por</h2>
        <v-list dense>
          <v-subheader class="font-weight-normal pl-4 text-subtitle-1">Estado</v-subheader>
          <v-list-item-group >
            <v-list-item v-for="(status, i) in aggregators.status" :key="i" @click="pushToURL(`status:${status.value}`)">
              <v-list-item-content>
                <v-list-item-title class="primary--text font-weight-light">
                  {{(status.value=='closed') ? 'Cerrado':'Abierto'}} ({{status.total}}) +
                </v-list-item-title>
              </v-list-item-content>
            </v-list-item>
          </v-list-item-group>
        </v-list>
        <v-list dense>
          <v-subheader class="font-weight-normal pl-4 text-subtitle-1">Salario deseado</v-subheader>
          <v-list-item-group >
            <v-list-item v-for="(status, i) in aggregators.compensationrange" :key="i" @click="pushToURL(`compensationrange:${status.value}`)">
              <v-list-item-content>
                <v-list-item-title class="primary--text font-weight-light">
                  {{status.value.replace('hourly','hora')}} ({{status.total}}) +
                </v-list-item-title>
              </v-list-item-content>
            </v-list-item>
          </v-list-item-group>
        </v-list>
        <v-list dense>
          <v-subheader class="font-weight-normal pl-4 text-subtitle-1">Tipo de trabajo</v-subheader>
          <v-list-item-group >
            <v-list-item v-for="(status, i) in aggregators.type" :key="i" @click="pushToURL(`type:${status.value}`)">
              <v-list-item-content>
                <v-list-item-title class="primary--text font-weight-light">
                  {{jobType(status.value)}} ({{status.total}}) +
                </v-list-item-title>
              </v-list-item-content>
            </v-list-item>
          </v-list-item-group>
        </v-list>
        <v-list dense>
          <v-subheader class="font-weight-normal pl-4 text-subtitle-1">Organizacion(es)</v-subheader>
          <v-list-item-group >
            <v-list-item v-for="(status, i) in organizations" :key="i" @click="pushToURL(`type:${status.value}`)">
              <v-list-item-content>
                <v-list-item-title class="primary--text font-weight-light">
                  {{status.value}} ({{status.total}}) +
                </v-list-item-title>
              </v-list-item-content>
            </v-list-item>
            <v-list-item>
              <v-list-item-content>
                <v-list-item-title class="primary--text font-weight-bold" @click="limitOrganizations+=6">
                  VER MAS
                </v-list-item-title>
              </v-list-item-content>
            </v-list-item>

          </v-list-item-group>
        </v-list>

      </v-navigation-drawer>
      <v-container v-if="$route.query.q">
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
      </v-container>
      <v-container fill-height align-center v-else>
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
        page: 1,
        jobs: {},
        aggregators: {
          organization:[]
        },
        limitOrganizations:6
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
        this.$axios.post(
          `https://search.torre.co/opportunities/_search/?offset=${this.offsetPage}&size=${this.sizePage}&page=${this.page}&aggregate=true`, {
            and: this.searchParams
          }).then((data) => {
          this.aggregators = data.data.aggregators
        })
      },
      jobType(type) {
        switch(type) {
          case 'full-time-employment':return 'Empleo a tiempo completo'
          break
          case 'freelance-gigs':return 'Trabajos temporales/freelance'
          break
          case 'part-time-employment':return 'Empleo a medio tiempo'
          break
          case 'internships':return 'Pasantias'
          break
        }
      },
      pushToURL(param) {
        let q = this.$route.query.q 
        q = `${q} AND ${param}`
        console.log(q)
        this.$router.push(`/search/jobs/?q=${q}`)
      }
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
      '$route.query.q':function(val) {
        this.findJobs()
      },
      organizations() {
        return this.aggregators.organization.slice(0,this.limitOrganizations)
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

  .search-filters-title {
    font-size: 20px;
    font-weight: 600;
    letter-spacing: .005em;
    line-height: 28px;
    background: hsla(0, 0%, 100%, .06);
    margin: 0;
    padding: 12px 16px;
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
