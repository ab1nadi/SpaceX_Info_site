<template>
<div id="container">
    <button v-on:click="All()">All Launches</button>
    <button v-on:click="Past()">Past Launches</button>
    <button v-on:click="Coming()">Coming Luances</button>
</div>
</template>

<script>

export default {
  name: 'Buttons',
  components: {

  },
  props: {
    All: Function,
    Past: Function,
    Coming: Function
  }
}

</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>



</style>
