<template>
  <div id="app">
  <router-view 
    v-on:matchClicked="onItemClicked" 
    v-on:removeMatch="onRemoveMatch"
    v-on:refreshMatchList="refreshMatchList"
    :matches="matches"
    :match="selectedMatch"
    :teams="selectedTeams"
    >
    </router-view>
  </div>
</template>

<script>
import Vue from 'vue';
import VueRouter from 'vue-router';
import numFormat from 'vue-filter-number-format';
import BootstrapVue from 'bootstrap-vue'

import axios, { AxiosPromise } from 'axios';

import List from './components/List.vue'
import Card  from './components/Card.vue'

Vue.filter('numFormat', numFormat);
Vue.use(VueRouter);
Vue.use(BootstrapVue)

export class OpenDotaClient {
    apiURL = "https://api.opendota.com/api"; 

    GetMatches() {
      return axios.get(`${this.apiURL}/proMatches`);
    }

    GetTeam(teamId) {
      return axios.get(`${this.apiURL}/teams/${teamId}`)
    }
}

export class Match {
  match_id;
}

const router = new VueRouter({
  mode: "history",
  routes: [
    { name: "card", path: "/card/:id", component: Card },
    { path: "*", component: List }
  ]
});

export default {
  name: 'app',
  router,
  components: {
    List,
    Card
  },
  data() {
    return {
      matches: [],
      selectedTeams: [],
      selectedMatch: {}
    }
  },
  mounted() {
    this.refreshMatchList();
  },
  methods: {
    onItemClicked(selectedMatch) {
      var client = new OpenDotaClient();
      Promise.all([
        client.GetTeam(selectedMatch.radiant_team_id), 
        client.GetTeam(selectedMatch.dire_team_id)
      ]).then(response => {
        this.selectedTeams = response.map(x => x.data);
      });   
      this.selectedMatch = selectedMatch;
      console.log(this.matches);
      router.push({name: 'card', params: {id: selectedMatch.match_id }});
    },
    onRemoveMatch(match) {
      var index = this.matches.indexOf(match);
      this.matches.splice(index, 1);
    },
    refreshMatchList() {
      var client = new OpenDotaClient();

      client.GetMatches()
        .then(response => {
          this.matches = response.data
        });
      }
  }
}
</script>
