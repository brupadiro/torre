<template>
  <v-container>
    <v-app-bar app class="elevation-1">
      <v-btn icon to="/search/jobs/">
        <v-icon>mdi-arrow-left</v-icon>
      </v-btn>
      <v-toolbar-title>{{job.objective}}</v-toolbar-title>
      <v-spacer></v-spacer>
      <v-btn icon>
        <v-icon color="primary">mdi-thumb-down</v-icon>
      </v-btn>
      <v-btn icon>
        <v-icon color="primary">mdi-thumb-up</v-icon>
      </v-btn>
      <v-btn icon>
        <v-icon color="primary">mdi-share</v-icon>
      </v-btn>

    </v-app-bar>
    <v-card flat>
      <v-img
        src="https://res.cloudinary.com/torre-technologies-co/image/upload/q_auto,c_fit,w_2000/v1593109931/dev/landing/uqngfnzme7cvvfovw8aw.jpg"
        height="200" width="100%">
        <div class="header-job">
          <h2>{{job.objective}}</h2>
          <p class="caption text-center font-weight-light mb-0 grey--text text--lighten-1 mb-0">
            {{jobType(job.opportunity)}}</p>
        </div>
      </v-img>
      <div class="pa-3">
        <v-card-text>
          <h3 class="primary--text font-weight-light mb-2">Habilidades y experiencia requeridas</h3>
          <div v-show="filterSkills('1-plus-year').length>0">
            <p class="grey--text text--lighten-1">+1 año de experiencia</p>
            <v-row>
              <v-chip v-for="skill in filterSkills('1-plus-year')" class="mr-2 font-weight-light">{{skill.name}}
              </v-chip>
            </v-row>
          </div>
          <div v-show="filterSkills('potential-to-develop').length>0">
            <p class="grey--text text--lighten-1">Debe tener interés aunque no experiencia</p>
            <v-row>
              <v-chip v-for="skill in filterSkills('potential-to-develop')" class="mr-2 font-weight-light">
                {{skill.name}}
              </v-chip>
            </v-row>
          </div>
        </v-card-text>
        <v-card-text>
          <h3 class="primary--text font-weight-light mb-2">Organization(s) name(s)</h3>
          <v-row no-gutters v-if="job.organizations.length > 0" class="d-flex align-center">
            <v-avatar size="60">
              <v-img :src="job.organizations[0].picture" width="100%"></v-img>
            </v-avatar>
            <span class="ml-3 white--text  caption text-subtitle-1">{{job.organizations[0].name}}</span>
          </v-row>
        </v-card-text>
        <v-card-text>
          <h3 class="primary--text font-weight-light mb-2">Location</h3>
          <v-row no-gutters v-if="job.organizations.length > 0" class="d-flex align-center">
            <v-icon color="primary" size="40">mdi-map-marker</v-icon>
            <v-chip class="font-weight-light ml-2">{{setPlace()}}</v-chip>
          </v-row>
        </v-card-text>
        <v-card-text>
          <h3 class="primary--text font-weight-light mb-2">Time zone</h3>
          <v-row no-gutters v-if="job.organizations.length > 0" class="d-flex align-center">
            <v-icon color="primary" size="40">mdi-earth</v-icon>
            <v-chip class="font-weight-light ml-2">{{setTimeZone()}}</v-chip>
          </v-row>
        </v-card-text>
        <v-card-text height="300">
          <v-row>
            <v-col class="col-12 col-md-12">
              <div class="compensation d-flex flex-column align-center">
                <h3 class="primary--text font-weight-light mb-3">Monetary compensation</h3>
                <p v-if="job.compensation" class="white--text text-h5 mb-0">
                  {{job.compensation.currency}} ${{job.compensation.minAmount}} -
                  {{job.compensation.maxAmount}}
                </p>
              </div>
            </v-col>
          </v-row>
        </v-card-text>
        <v-card-text>
          <v-row>
            <v-col class="col-md-4 col-sm-4 col-12" v-for="member in job.members">
              <v-card class="rounded-lg team-card" flat>
                <v-card-title class="d-flex justify-center">
                  <span class="caption text-center font-weight-light grey--text text--lighten-1">You’d be working
                    with<br>(listing manager):</span>
                </v-card-title>
                <v-card-text class="d-flex justify-center">
                  <v-avatar size="80" color="grey">
                    <v-img :src="member.person.picture" width="100%"></v-img>
                  </v-avatar>
                </v-card-text>
                <v-card-text>
                  <h3 class="text-center primary--text font-weight-light mb-3">{{member.person.name}}</h3>
                  <p class="text-center member-team-title font-weight-light">{{member.person.professionalHeadline}}</p>
                </v-card-text>
                <v-card-text class="text-center">
                  <v-row no-gutters>
                    <v-col class="col-12">
                      <v-btn color="primary" class="black--text">
                        <v-icon>mdi-human-greeting</v-icon>SIGNAL
                      </v-btn>
                    </v-col>
                    <v-col class="col-12">
                      <v-btn text color="primary" class="font-weight-light">VIEW GENOME</v-btn>
                    </v-col>
                  </v-row>
                </v-card-text>
              </v-card>
            </v-col>
          </v-row>
        </v-card-text>
      </div>
      <v-card-text v-for="detail in job.details" class="detail-container mb-6">
        <h3 class="primary--text font-weight-light mb-2">{{titleDetails(detail.code)}}</h3>
        <p class="detail-content font-weight-light">{{detail.content}}</p>
      </v-card-text>
      <v-card-text class="owner-info detail-container">
        <v-row no-gutters class="d-flex align-center">
          <v-col class="col-md-2 col-2 d-flex justify-center">
            <v-avatar size="60">
              <v-img :src="job.owner.picture" width="100%"></v-img>
            </v-avatar>
          </v-col>
          <v-col class="col-md-10 col-10">
            <h3 class="primary--text font-weight-light mb-3">{{job.owner.name}}</h3>
            <p class="member-team-title font-weight-light">{{job.owner.professionalHeadline}}</p>

          </v-col>
        </v-row>
      </v-card-text>
    </v-card>
  </v-container>
</template>

<script>
  export default {
    data() {
      return {
        jobId: this.$route.params.job_id,
        job: {
          strengths: [],
          organizations: [],
          owner: {}
        }
      }
    },
    created() {
      this.getJob()
    },
    methods: {
      getJob() {
        this.$axios.get(`https://torre.co/api/opportunities/${this.jobId}`)
          .then((data) => {
            this.job = data.data
          })
      },
      filterSkills(type) {
        return this.job.strengths.filter((strength) => {
          console.log(strength.experience, type)
          if (strength.experience == type) {
            return strength
          }
        })
      },
      jobType(type) {
        switch (type) {
          case 'full-time-employment':
            return 'Empleo a tiempo completo'
            break
          case 'freelancer':
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
      setPlace() {
        if (this.job.place.remote) {
          return 'Remote'
        } else {
          return this.job.place.location[0].id
        }
      },
      setTimeZone() {
        if (this.job.place.timezone) {
          return 'Remote'
        } else {
          return 'Any time zone'
        }
      },
      titleDetails(code) {
        switch (code) {
          case 'reason':
            return 'Why this opportunity exists'
            break
          case 'responsibilities':
            return 'Responsibilities'
            break
          case 'requirements':
            return 'Additional requirements (other than skills)'
            break
          case 'organizations':
            return 'About the organization(s)'
            break
          case 'additional':
            return 'Additional information'
            break
        }

      }

    }
  }

</script>

<style scoped>
  .header-job {
    position: absolute;
    bottom: 0;
    left: 0;
    right: 0;
    display: inline-block;
    width: 50%;
    margin: 0 auto;
    background: #1e1e1e;
    padding: 10px;
    border-top-left-radius: 20px;
    border-top-right-radius: 20px;
  }

  .header-job h2 {
    text-align: center;
    font-size: 20px;
    font-weight: bold;
  }

  .compensation {
    height: 150px;
    display: flex;
    justify-content: center;
    align-items: center;
    background: #555555;
    border-radius: 28px !important;
  }

  .team-card {
    background: #555555;
  }

  .member-team-title {
    white-space: nowrap;
    overflow: hidden;
    text-overflow: ellipsis;

  }

  .detail-content {
    white-space: pre-wrap;
  }

  .detail-container {
    background: linear-gradient(180deg, rgba(205, 220, 57, .06), rgba(205, 220, 57, 0) 50%)
  }

</style>
