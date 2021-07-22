<template>
<div id="container">

  <table class="scrollheaderTable"> 
    <th>Launch Name</th>
    <th>Rocket Name</th>
    <th>Date</th>
    <th>Success</th>
    </table>

  <table class="LaunchTable">

       <tr v-for="(item) in dataShown "  :key="item.id">
                <td style="width=20xp">{{item.name}}</td>
                <td>{{item.rocketName}}</td>
                <td>{{item.date}}</td>
                <td>{{item.successString}}</td>
        </tr>
  </table>

  </div>
</template>

<script>
import axios from 'axios'
import lodash from 'lodash'
import VueLodash from 'vue-lodash'
import Vue from 'vue'

Vue.use(VueLodash, { name: 'custom' , lodash: lodash })

export default {
  name: 'Launchlist',
  components: {
    
  },
  props: {
    api: String,
    SearchString: String,
    Get: Boolean
  },
  data() {
    return {data: null,
            dataShown: null}
  },
  beforeMount: function () {
      let vm = this;
      // go get the rocket name with axios
      // then pump it into the table data
    
      // if we are told to get
      axios.get(vm.api)
      .then(response => {
        
        vm.data = response.data;



        for(let i = 0; i<vm.data.length; i++)
        {
          vm.data[i].date = toDateString(vm.data[i].date_unix);
          
         if(vm.data[i].success == undefined || vm.data[i].success == null )
            vm.data[i].successString = "Launch Success Imminent";
          else if(vm.data[i].success)
            vm.data[i].successString = "Success";
          else 
            vm.data[i].successString = "Unsuccessful";


          let z = vm.data;

          vm.rocketName = "";

          axios.get("https://api.spacexdata.com/v4/rockets/" +z[i].rocket)
          .then(response => {
            if(response.data.name != null && response.data.name != undefined)
            z[i].rocketName = response.data.name;
            else 
            z[i].rocketName = "";

            this.filter();

            vm.$forceUpdate();
            });

          vm.data;
        }

          // hopefully the filter will catch the incoming rocketnames but who knows
        });

  }

, 

methods:
{
  filter: function()
  {

    let vm = this;

  if(vm.SearchString != "")
   vm.dataShown =  vm._.filter(
    vm.data, function(item) {

       if( item.name.includes(vm.SearchString) || (item.rocketName != undefined && item.rocketName.includes(vm.SearchString)) || item.date.includes(vm.SearchString) || item.successString.includes(vm.SearchString))
       return item;

    }

);

else 
vm.dataShown = vm.data;

      
  }
}



}

// toDateString, converst unix time to mm/dd/yyyy
 function toDateString(unixTime)
  {

  var date = new Date(unixTime*1000);
  var year = date.getFullYear();
  var month = date.getMonth();
  var day = date.getDate();

// I would use the template syntax but I cannot figure it out.
  return month + "/" + day + "/" + year;

  }

</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>

#container 
{
  position: relative;
  display:block;
  height: 500px;
  overflow-y: scroll;
  width: 1000px;
}

table {
    table-layout: fixed;
    width: 100%;
    border:1px solid;
    border-collapse: collapse;
}


td {
    min-width: 250px;
    border:1px solid;
    text-align:left;
    padding-left: 3px;

}

th 
{
  text-align: left;
  padding-left: 3px;
}

.scrollheaderTable
{
  height: 0;
  background-color:white;
  overflow-y:unset;
  position: -webkit-sticky; /* Safari */
  position: sticky;
  top: 0;
}

</style>
