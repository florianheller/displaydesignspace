<template>
  <div id="app">
    <BodyMap v-on:body-area-selected="onBodyAreaSelected($event)"></BodyMap>
    <DataFilter></DataFilter>
    <ItemDisplay></ItemDisplay>
  </div>
</template>

<script>
  import BodyMap from './components/BodyMap.vue'
  //import DataDisplay from './components/DataDisplay.vue'
  import DataFilter from './components/DataFilter.vue'
  import ItemDisplay from './components/ItemDisplay.vue'


  export default {
    name: "DesignSpaceApp",
    // methods: {
    //   onBodyAreaSelected: function (area) {
    //     this.DataDisplay.setActiveArea(area);
    //   }
    // },
    components: {
      BodyMap: BodyMap,
      DataFilter: DataFilter,
      ItemDisplay: ItemDisplay
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
  margin-top: 60px;
  width:100%;
  display:flex;
  justify-content:space-between;
}
</style>
