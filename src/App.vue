<template>
  <div id="app">
    <Buttons v-bind:All="this.all" v-bind:Past="this.past" v-bind:Coming="this.coming"/>
     
    <h1>{{current_header}}</h1>

    <SearchBar @inputChange="searchList" id="search" />

    <LuanchList v-bind:Get="get" v-bind:SearchString="searchText" id="list" v-bind:api="current_api" :key="searchText+current_api"/>
    <div id="apiInfo">Current API being consumed: {{current_api}}</div>
    <div id="update"> Updating In: {{updateCount}}</div>
  </div>
</template>

<script>
import LuanchList from './components/LuanchList.vue'
import Buttons from './components/Buttons.vue'
import SearchBar from './components/SearchBar.vue'

export default {
  name: 'App',
  components: {
    LuanchList,
    Buttons,
    SearchBar
  }
  ,
  data() {
    return {
      current_api: "https://api.spacexdata.com/v4/launches",
      current_header: "All Launches",
      api_all: "https://api.spacexdata.com/v4/launches",
      api_upcoming: "https://api.spacexdata.com/v4/launches/upcoming",
      api_past: "https://api.spacexdata.com/v4/launches/past",
      updateCount: 30,
      timeToUpdate: 30,
      searchText: ""
      , get: true
      }
  },

  beforeMount: function () {

     this.apiCount();

  },
  methods : {
  
  all: function() {
    this.current_api = this.api_all;
     this.updateCount = this.timeToUpdate;
    this.current_header = "All Launches";
    this.get=true;
    this.$forceUpdate();
  }, 
  
  past: function()
  {
    this.current_api = this.api_past;
     this.updateCount = this.timeToUpdate;
    this.current_header = "Past Launches";
    this.get=true;
    this.$forceUpdate();
  }, 

  coming: function()
  {
    this.current_api = this.api_upcoming;
     this.updateCount = this.timeToUpdate;
    this.current_header = "Up Coming Launches";
    this.get=true;
    this.$forceUpdate();
  }, 
  apiCount: function()
  {
    let vm = this;
    setInterval(function()
    {
      if(vm.updateCount != 0)
        vm.updateCount--;
      else 
      {
        vm.updateCount = vm.timeToUpdate;
        vm.$forceUpdate();
      }
    }, 1000);
  },

  // sets the stored search text
  // then searches
  searchList: function(searchText_)
  {
    this.searchText = searchText_;
    this.get=false;
    this.$forceUpdate();
  }

}
  

}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}

#list 
{
  margin-left: auto;
  margin-right: auto;
  width: 900px;
  overflow-y: scroll;
  height: 500px;
}

#search
{
  width: 900px;
  margin-left: auto;
  margin-right: auto;
  text-align:right;
}

#apiInfo 
{
  margin-top: 20px;
  width: 900px;
  margin-right: auto;
  margin-left: auto;
}
</style>
