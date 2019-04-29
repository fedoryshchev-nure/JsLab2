<template >
  <div class="container">
    <b-card
      class="text-center"
      v-bind:title="`Match id : ${match.match_id}`"
    >
      <b-card-text>
        <p> Duration : {{(match.duration / 60) | numFormat }} minutes </p>
        <p> Radiiant : {{match.radiant_name}} </p>
        <p> Dire : {{match.dire_name}} </p>
        <p> Winner : {{match.radiant_win ? 'Radiant' : 'Dire'}} </p>
      </b-card-text>
    </b-card>
    <br>
    <div class="row">
      <div 
      class="col-6 text-center"
      v-for="team of teams" 
      v-bind:key="team.team_id">
        <b-card
        v-bind:title="`Team : ${team.name}`"
        v-bind:img-src="team.logo_url"
        >
          <b-card-text > 
            <p> Rating : {{team.rating}} </p>
            <p> Wins : {{team.wins}} </p>
          </b-card-text>
        </b-card>
      </div>
    </div>
  </div>
</template>

<script lang="js">
  export default  {
    name: 'Card',
    props: {
      match: {
        type: Object
      },
      teams: {
        type: Array,
        default: []
      }
    },
}
</script>
