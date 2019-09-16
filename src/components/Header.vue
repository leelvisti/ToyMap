<template>
  <div id="header">
    <nav class="navbar navbar-light bg-light">
      <!-- prevent form refreshing after pressing enter-->
      <form class="form-inline" onsubmit="return false">
        <!-- add new location when search occurs -->
        <input
          class="form-control mr-sm-2"
          type="search"
          id="address-input2"
          placeholder="Search location"
          aria-label="Search">
        <button class="btn btn-outline-primary my-2 my-sm-0" id="searchLocation" type="button">Search</button>
      </form>
    </nav>
    <NavigationElements
      :locations="markedLocations"
      @locate="locate"
      @updateLocations="updateLocations"/>
  </div>
</template>

<script>
import NavigationElements from './NavigationElements.vue'

export default {
  name: 'Header',
  components: {
    NavigationElements
  },
  components: {
    NavigationElements
  },
  data () {
    return {
      markedLocations: this.locations
    }
  },
  props: {
    locations: {
      type: Array,
      required: true
    }
  },
  methods: {
    // send to App.vue to locate clicked place
    locate (index) {
      this.$emit('locate', index)
    }, // locatePlace
    updateLocations (updatedLocations) {
      this.$emit('updateLocations', updatedLocations)
    } // updateLocations
  }
}
</script>

<style scoped>
#header {
  width: 100vw;
}
</style>
