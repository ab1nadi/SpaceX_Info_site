<template>
  <span>
   {{rocketName}}
  </span>
</template>

<script>
import axios from 'axios'

export default {
  name: 'App',
  props: {
    RocketId: String
  }, 
    data() {
    return {rocketName: null}
  }, 
  beforeMount: function () {
      let vm = this;


        if(vm.RocketId != undefined)
        axios.get("https://api.spacexdata.com/v4/rockets/" +vm.RocketId)
      .then(response => {
        vm.rocketName = response.data.name;

        });
        else
        vm.rocketName = "Unknown";

        
  }
}
</script>

<style scoped>

</style>
