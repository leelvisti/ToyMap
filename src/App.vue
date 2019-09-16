<template>
  <div id="app" :style="[windowWidth < 700 ? blockDisplay : flexDisplay]">
    <!-- if screen width is larger than a mobile device, have sidebar -->
    <Sidebar
      :locations="locations"
      @locate="locate"
      @updateLocations="updateLocations"
      v-if="windowWidth > 700"></Sidebar>
    <!-- if screen width is similar to mobile device, have header -->
    <Header
      :locations="locations"
      @locate="locate"
      @updateLocations="updateLocations"
      v-else></Header>
    <Map 
      :locations="locations"
      :locatedPlace="locatedPlace"></Map>
  </div>
</template>

<script>
import Map from './components/Map.vue'
import Header from './components/Header.vue'
import Sidebar from './components/Sidebar.vue'

export default {
  name: 'App',
  components: {
    Header,
    Map,
    Sidebar
  },
  created() {
    window.addEventListener('resize', this.handleResize)
    this.handleResize();
  },
  destroyed() {
    window.removeEventListener('resize', this.handleResize)
  },
  data () {
    return {
      blockDisplay: {
        display: 'block'
      },
      flexDisplay: {
        display: 'flex'
      },
      locatedPlace: null,
      // locations are to be set with markers
      locations: [
        {
          label: 'Home 1',
          latitude: 37.618770,
          longitude: -122.071080
        },
        {
          label: 'Home 2',
          latitude: 37.660760,
          longitude: -122.100520
        },
        {
          label: 'Home 3',
          latitude: 37.655670,
          longitude: -122.099210
        }
      ],
      windowWidth: 0,
    }
  },
  methods: {
    // find the clicked location from navigation list
    locate (index) {
      this.locatedPlace = this.locations[index]
    }, // locate
    // handle window width when resized
    handleResize() {
      this.windowWidth = window.innerWidth;
    }, // handleResize
    // update when changes has occured to locations list
    // pass to children components
    updateLocations (updatedLocations) {
      this.locations = updatedLocations
    } // updateLocations
  }
}
</script>

<style>
/* remove any margin and paddings */
* {
  margin: 0;
  padding: 0;
}

#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  height: 100vh; /* fit whole window screen */
}

</style>
