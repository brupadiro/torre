<template>
  <v-input width="100%" hide-details rounded>
    <v-menu offset-y>
      <template v-slot:activator="{ on, attrs }">
        <v-combobox dense prepend-inner-icon="mdi-briefcase-search" @change="updateLastChip()" class="search-input rounded-pill" rounded return-object
        hide-details v-model="tags" chips clearable :label="(checkLabelCondition) ? label:''"
          v-on="on" outlined multiple>
          <template v-slot:selection="{ attrs, item, select, selected }">
            <v-chip v-bind="attrs" :input-value="selected" close small class="font-weight-light">
              <strong>{{ item }}</strong>&nbsp;
            </v-chip>
            <span class="ml-2 white--text">AND</span>
            <span class="ml-2 white--text overline">{{searchArea}}</span>
          </template>
          <template v-slot:prepend-inner>
            <span class="ml-2 white--text overline" v-if="tags.length == 0 && searchArea">
              {{searchArea}}
            </span>
          </template>
        </v-combobox>
      </template>
     <slot :setSearchArea="setSearchArea"></slot>
    </v-menu>
  </v-input>
</template>

<script>
  export default {
    props:{
      label:String,
    },
    data() {
      return {
        search: {

        },
        tags: [],
        searchArea: ''
      }
    },
    mounted(){
      if(this.$route.query.q) {
        this.tags = this.$route.query.q.split(' AND ')
      }
    },
    methods: {
      setSearchArea(area) {
        this.searchArea = area
      },
      updateLastChip() {
        let indexLast = this.tags.length - 1
        let chipValue = ""
        if (this.searchArea != "") {
          chipValue = `${this.searchArea} ${this.tags[indexLast]}`
          this.searchArea = ""
        } else {
          chipValue = `Job/Skill: ${this.tags[indexLast]}`
        }
        this.$set(this.tags, indexLast, chipValue)
        
        this.setSearchParams()
      },
      setSearchParams() {
        let path = this.tags.join(' AND ')
        if(this.label.indexOf('trabajos')!=-1){
          this.$router.push(`/search/jobs/?q=${path}`)
        } else {
          this.$router.push(`/search/people/?q=${path}`)
        }
      }
    },
    computed: {
      checkLabelCondition() {
        return (this.searchArea == '' && this.tags.length == 0)
      }
    },
    watch: {
      '$route.query.q':function(val) {
        this.tags = val.split(' AND ')
      },
      tags:{
        handler(val) {

        },  
        deep:true
      }
    },
  }

</script>
<style>
  .search-input {
    width: 90%;
  }

</style>
