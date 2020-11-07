<template>
  <v-input width="100%" hide-details rounded>
    <v-menu offset-y>
      <template v-slot:activator="{ on, attrs }">
        <v-combobox dense prepend-inner-icon="mdi-briefcase-search" @change="updateLastChip()" class="search-input rounded-pill" rounded :prepend-inner="'wola'" return-object
        hide-details v-model="tags" chips clearable :label="(checkLabelCondition) ? 'Buscar trabajos':''"
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
      <v-list>
        <v-list-item>
          <v-list-item-title class="font-weight-light" @click="searchArea = 'Job/Skill:'">Job/Skill:
          </v-list-item-title>
        </v-list-item>
        <v-list-item>
          <v-list-item-title class="font-weight-light" @click="searchArea = 'Organization:'">Organization:
          </v-list-item-title>
        </v-list-item>
      </v-list>
    </v-menu>
  </v-input>
</template>

<script>
  export default {
    data() {
      return {
        search: {

        },
        editable: [],
        tags: [],
        searchArea: ''
      }
    },
    methods: {
      searchItems() {
        this.$axios.post()
      },
      indexEditable(item) {
        return this.tags.indexOf(item)
      },
      editChip(item) {
        let index = this.indexEditable(item)
        this.$set(this.editable, index, true)

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

      }
    },
    computed: {
      checkLabelCondition() {
        return (this.searchArea == '' && this.tags.length == 0)
      }
    }
  }

</script>
<style>
  .search-input {
    width: 90%;
  }

</style>
