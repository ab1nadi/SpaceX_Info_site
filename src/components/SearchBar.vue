<template>
  <div id="searchBar">
      Search: <input v-model="searchText" @input="onInput">
  </div>
</template>

<script>

export default {
  name: 'SearchBar',
    data()
    {
        return {
            searchText: null
        }
    },

    methods:
    {
        onInput: function()
        {
           this.$emit('inputChange', this.searchText);
        }
    }
 }
</script>

<style scoped>
#searchBar  > *
{
    float: right;
}

</style>
