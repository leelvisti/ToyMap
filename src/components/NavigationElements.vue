<template>
  <div id="list-container" class="bg-light">
    <h4>Navigation List</h4>
    <div
      class="text-left location-item"
      v-for="(location, index) in markedLocations"
      :key="index"
      :style="[location.label.length > 12 ? blockDisplay : flexDisplay]">
      <h5 class="mt-sm-2">{{ location.label }}</h5>
      <div class="ml-auto">
        <!-- when clicked, pan map to the designated location -->
        <button type="button" class="btn btn-outline-primary" @click="locate(index)">
          Locate
        </button>
        <!-- remove location from array -->
        <button type="button" class="btn btn-outline-danger" @click="removeLocation(index)">
          Remove
        </button>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'NavigationElement',
  data () {
    return {
      markedLocations: this.locations,
      blockDisplay: {
        display: 'block'
      },
      flexDisplay: {
        display: 'flex'
      }
    }
  },
  props: {
    locations: {
      type: Array,
      required: true
    }
  },
  methods: {
    // locate location on map
    locate (index) {
      this.$emit('locate', index)
    }, // locate
    // remove location from list
    removeLocation (index) {
      this.markedLocations.splice(index, 1)
      this.$emit('updateLocations', this.markedLocations)
    } // removeLocation
  }
}
</script>

<style scoped>
.location-item {
  background-color: #fff;
  padding: 10px;
  border-bottom: 1.5px solid #f8f9fa;
  display: flex; /* have elements side by side */
}
</style>
