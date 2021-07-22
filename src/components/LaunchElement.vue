<template>
  <tr>
      <td>{{data.name}}</td>
      <td>{{rocketName}}</td>
      <td>{{data.date_utc}}</td>
      <td>{{this.data.success}}</td>
      <td><a v-bind:href="data.webcast">Open Web Cast</a></td>
  </tr>
</template>

<script>
import axios from 'axios'

export default {
  name: 'LaunchElement',
  props: {
    data: Object
  },
  data() {
    return { rocketName: "dunno" }
  },

  mounted: function () {
      let vm = this;

      console.log(this.data)

      axios.get("https://api.spacexdata.com/v4/rockets/:" + vm.data.rocket)
      .then(response => (vm.data = response.name));

      // go get the rocket name with axios
      // then pump it into the table data
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>

</style>
