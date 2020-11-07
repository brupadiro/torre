<template>
  <v-card>
    <v-card-text>
      <v-row no-gutters>
        <v-col class="col-md-9 col-9">
          <h5 class="primary--text font-weight-light mb-2 text-subtitle-1">{{job.objective}}</h5>
          <span class="font-weight-light grey--text text--lighten-1 mb-2">{{job.type}}</span><br>
          <span class="font-weight-light grey--text text--lighten-1" v-if="job.organizations[0]">{{job.organizations[0].name}}</span>
          <p v-if="job.compensation && job.compensation.data" class="white--text font-weight-bold">
            {{job.compensation.data.currency}} ${{job.compensation.data.minAmount}} -
            {{job.compensation.data.maxAmount}}
          </p>
          <span class="font-weight-light grey--text text--lighten-1">{{(job.status=='Open')?'Abierto':'Cerrado'}}</span>
          <div class="d-flex mt-3">
            <v-icon color="primary" size="20">mdi-earth</v-icon>
            <span class="font-weight-light ml-2">{{(job.locations.length==0) ? 'Remoto':job.locations[0]}}</span>
          </div>
        </v-col>
        <v-col class="col-md-3 col-3">
          <v-avatar class="float-right">
            <v-img v-if="job.organizations[0]" :src="job.organizations[0].picture" width="100%"></v-img>
          </v-avatar>
        </v-col>
      </v-row>
    </v-card-text>
    <v-card-actions>
      <v-menu open-on-hover transition="slide-x-transition" bottom right offset-y>
        <template v-slot:activator="{ on, attrs }">
          <v-btn icon v-bind="attrs" v-on="on">
            <v-icon color="primary">mdi-dots-vertical</v-icon>
          </v-btn>
        </template>
        <v-card class="mx-auto" max-width="300" tile>
          <v-list dense>
            <v-list-item color="primary">
              <v-list-item-content>VER PREGUNTAS</v-list-item-content>
            </v-list-item>
          </v-list>
        </v-card>
      </v-menu>

      <v-spacer></v-spacer>
      <v-btn class="ml-2">
        <v-icon class="primary--text" size="16">mdi-thumb-down</v-icon>
      </v-btn>
      <v-btn class="ml-2">
        <v-icon class="primary--text" size="16">mdi-thumb-up</v-icon>
      </v-btn>
      <v-btn class="ml-2 black--text lighen-1" color="primary" :to="`/jobs/${job.id}`">VER</v-btn>
    </v-card-actions>
  </v-card>

</template>

<style scoped>
  .skill {
    padding: 2px;
    border: 1px solid #ffffff73;
    margin-right: 4px;
  }

  .card-skills {
    overflow: hidden;
    position: relative;
    white-space: nowrap;
    width: 100%
  }

  .card-skills::after {
    background: linear-gradient(90deg, rgba(39, 41, 45, 0), #27292d);
    bottom: 0;
    content: "";
    display: block;
    position: absolute;
    right: 0;
    top: 0;
    width: 20%;
  }

</style>

<script>
  export default {
    props: {
      job: Object
    }
  }

</script>
