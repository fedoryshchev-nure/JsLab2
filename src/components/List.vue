<template >
  <div class="container">
    <div class="btn btn-primary" @click="$emit('refreshMatchList')">
      Refresh match list
    </div>
    <br>
    <br>
    <div >
      <ul v-if="matches && matches.length">
        <div
        class="mb-4"
        v-for="match of matches" 
        v-bind:key="match.match_id"
        >
        <li> 
          <p> 
            <span @click="$emit('matchClicked', match)">
              Match id : {{match.match_id}}
              &nbsp;
              Winner : {{match.radiant_win ? 'Radiant' : 'Dire'}}
            </span>
            &nbsp;
            <span class="btn btn-danger" @click="$emit('removeMatch', match)"> 
              Remove match
            </span>
          </p>
          <br>   
        </li>
      </div>  
      </ul>
    </div>
  </div>
</template>

<script lang="js">
  export default  {
    name: 'List',
    props: {
      matches: {
        type: Array, 
        default: () => []
      }
    }
}
</script>

