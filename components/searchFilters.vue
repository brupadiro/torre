<template>
  <div class="fill-height">
    <div v-if="loadingAggregators" class="d-flex align-center justify-center fill-height">
      <v-progress-circular indeterminate color="primary" size="100"></v-progress-circular>
    </div>
    <div v-else>
      <v-toolbar elevation="0" color="grey darken-4">
        <v-btn icon @click="closeFilters()" class="hidden-md-and-up">
          <v-icon>mdi-close</v-icon>
        </v-btn>
        <v-toolbar-title class="search-filters-title">Buscar trabajos por</v-toolbar-title>
      </v-toolbar>
      <v-list dense v-show="!findCoincidencesInSearch('status')">
        <v-subheader class="font-weight-normal pl-4 text-subtitle-1">Estado</v-subheader>
        <v-list-item-group>
          <v-list-item v-for="(status, i) in aggregators.status" :key="i" @click="pushToURL(`status:${status.value}`)">
            <v-list-item-content>
              <v-list-item-title class="primary--text font-weight-light">
                {{(status.value=='closed') ? 'Cerrado':'Abierto'}} ({{status.total}}) +
              </v-list-item-title>
            </v-list-item-content>
          </v-list-item>
        </v-list-item-group>
      </v-list>
      <v-list dense v-show="!findCoincidencesInSearch('compensationrange')">
        <v-subheader class="font-weight-normal pl-4 text-subtitle-1">Salario deseado</v-subheader>
        <v-list-item-group>
          <v-list-item v-for="(status, i) in aggregators.compensationrange" :key="i"
            @click="pushToURL(`compensationrange:${status.value}`)">
            <v-list-item-content>
              <v-list-item-title class="primary--text font-weight-light">
                {{status.value.replace('hourly','hora')}} ({{status.total}}) +
              </v-list-item-title>
            </v-list-item-content>
          </v-list-item>
        </v-list-item-group>
      </v-list>
      <v-list dense v-show="!findCoincidencesInSearch('type')">
        <v-subheader class="font-weight-normal pl-4 text-subtitle-1">Tipo de trabajo</v-subheader>
        <v-list-item-group>
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
        <v-list-item-group>
          <v-list-item v-for="(status, i) in organizations" :key="i" @click="pushToURL(`organization:${status.value}`)">
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
    </div>
  </div>
</template>

<script>
  export default {
    props: {
      aggregators: Object,
      loadingAggregators: Boolean
    },
    data() {
      return {
        limitOrganizations: 6
      }
    },
    methods: {
      //Set job type by type
      jobType(type) {
        switch (type) {
          case 'full-time-employment':
            return 'Empleo a tiempo completo'
            break
          case 'freelance-gigs':
            return 'Trabajos temporales/freelance'
            break
          case 'part-time-employment':
            return 'Empleo a medio tiempo'
            break
          case 'internships':
            return 'Pasantias'
            break
        }
      },
      pushToURL(param) {
      //Pushing params to url
        let q = this.$route.query.q
        if (q) {
          q = `${q} AND ${param}`
        } else {
          q = param
        }
        this.$router.push(`/search/jobs/?q=${q}`)
        this.$emit('closeFilters',true)
      },
      findCoincidencesInSearch(param) {
      //Finding previous param in url
        return (this.$route.query.q!=undefined && this.$route.query.q.indexOf(param) != -1)
      },
      closeFilters() {
        this.$emit('closeFilters',true)
      }
    },
    computed: {
      organizations() {
        return this.aggregators.organization.slice(0, this.limitOrganizations)
      }
    }
  }

</script>

<style>
  .search-filters-title {
    font-size: 20px;
    font-weight: 600;
    letter-spacing: .005em;
    line-height: 28px;
    margin: 0;
    padding: 12px 16px;
  }

</style>
