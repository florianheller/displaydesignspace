<template>
  <div id="app">
    <BodyMap v-on:body-area-selected="onBodyAreaSelected($event)"></BodyMap>
    <DataDisplay></DataDisplay>
  </div>
</template>

<script>
  import BodyMap from './components/BodyMap.vue'
  //import DataDisplay from './components/DataDisplay.vue'
  import DataFilter from './components/DataFilter.vue'


  export default {
    name: "DesignSpaceApp",
    methods: {
      onBodyAreaSelected: function (area) {
        // eslint-disable-next-line no-console
        console.log('App got an area ' + area );
      }
    },
    components: {
      BodyMap: BodyMap,
      DataDisplay: DataFilter
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
}
</style>
