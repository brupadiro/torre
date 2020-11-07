<template>
  <v-card>
    <v-card-text>
      <v-row no-gutters>
        <v-col class="col-md-12 col-12">
          <v-row no-gutters>
            <v-col class="col-2 col-md-2">
              <v-avatar>
                <v-img v-if="job.organizations[0]" :src="job.organizations[0].picture" width="100%"></v-img>
              </v-avatar>
            </v-col>
            <v-col class="col-6 col-md-6">
              <h5 class="primary--text font-weight-light mb-2">{{job.objective}}</h5>
              <span class="font-weight-light grey--text text--lighten-1  caption mb-2">{{job.type}}</span>
              <span class="font-weight-light grey--text text--lighten-1  caption" v-if="job.organizations[0]">{{job.organizations[0].name}}</span>
            </v-col>
            <v-col class="col-md-4 col-4"></v-col>
          </v-row>
        </v-col>


        <v-col class="col-md-12 col-12">
          <v-row no-gutters>
            <v-col class="col-2 col-md-2">
            </v-col>
            <v-col class="col-10 col-md-10 mt-2">
              <p v-if="job.compensation && job.compensation.data" class="white--text font-weight-bold">
                {{job.compensation.data.currency}} ${{job.compensation.data.minAmount}} -
                {{job.compensation.data.maxAmount}}
              </p>
              <div class="card-skills d-inline-flex mb-3">
                <div class="skill" v-for="(skill,index) in job.skills" :key="index + job.id">
                  <span class="font-weight-light white--text caption">{{skill.name}} - 1+ year</span>
                </div>
              </div>
              <div class="status">
                <span class="grey--text text--lighten-1 font-weight-light">{{(job.status=='closed')? 'Cerrado':'Abierto'}}</span>
            </div>
        </v-col>
          </v-row>
        </v-col>
        <v-col class="col-md-12 col-12 mt-6">
          <v-row no-gutters>
            <v-col class="col-3 col-md-3">
            </v-col>
            <v-col class="col-9 col-md-9 d-inline-flex justify-end">
                <v-btn class="ml-2">
                    <v-icon class="primary--text" size="16">mdi-thumb-down</v-icon>
                </v-btn>
                <v-btn class="ml-2">
                    <v-icon class="primary--text" size="16">mdi-thumb-up</v-icon>
                </v-btn>
                <v-btn class="ml-2 primary--text">HACER UNA PREGUNTA</v-btn>
                <v-btn class="ml-2 black--text lighen-1" color="primary" :to="`/jobs/${job.id}`">VER</v-btn>
            </v-col>
          </v-row>
        </v-col>
      </v-row>
    </v-card-text>
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
    width:100%

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
