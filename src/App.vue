<template>
  <div id="app">
    <div id="header"><a href="license.html">License</a></div>
    <BodyMap v-on:body-area-selected="onBodyAreaSelected($event)" :filteredData="filteredData"></BodyMap>
    <DataFilter v-model="filteredData"></DataFilter>
    <ItemDisplay></ItemDisplay>
    <div id="footer">
      <img src="icons/Aalto.svg" height="35" class="logo" /><br/>
      <img src="icons/UHasselt.svg" height="35" class="logo" />
      <img src="icons/FM.svg" height="35" class="logo" /> 
    </div>
  </div>
</template>

<script>
  import BodyMap from './components/BodyMap.vue'
  import DataFilter from './components/DataFilter.vue'
  import ItemDisplay from './components/ItemDisplay.vue'


  export default {
    name: "DesignSpaceApp",
    components: {
      BodyMap: BodyMap,
      DataFilter: DataFilter,
      ItemDisplay: ItemDisplay
    },
    data: function()  {
      return {
        filteredData: []
      }
    }
  };
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  width:100%;
  display:flex;
  justify-content:space-between;
}
#header a {
  color:#F5F6F8;
  text-decoration:none;
  margin-right:10px;
}

#footer {
  position: fixed;
  left: 0;
  bottom: 0;
  height: 80px;
  text-align: left;
  padding-bottom: 5px;
}

#header {
  position: fixed;
  left: 0;
  top: 0;
  width: 100%;
  padding: 5px;
  background-color: #384c6c;
  color: white;
  text-align: right;
}

.logo {
  margin-left: 10px;
  padding: 2px;
}
</style>
