<template>
  <div id="map">
  </div>
</template>

<script>
export default {
  name: 'Map',
  props: {
    locations: {
      type: Array,
      required: true
    },
    locatedPlace: {
      type: Object,
      required: false
    }
  },
  data () {
    return {
      autocomplete: null,
      // stores locations to be rendered with marker
      markedLocations: this.locations,
      map: null, // actual google map,
      geocoder: null,
      geocodeAddress: this.geocodeQuery,
      bounds: null, // viewpoints of what is visible
      markers: [] // stores all markers
    }
  },
  methods: {
    // set up google maps
    initGoogleMap () {
      this.bounds = new google.maps.LatLngBounds()
      const center = this.markedLocations[0]
      // have map be ready to render when component is live
      const element = document.getElementById('map')
      const options = {
        zoom: 12,
        center: new google.maps.LatLng(center.latitude, center.longitude)
      }

      this.geocoder = new google.maps.Geocoder();
      this.map = new google.maps.Map(element, options)
    }, // initGoogleMap
    // take locations from markedLocations and add markers to each
    setMarkers () {
      // iterate through all marked locations to set marker
      this.markedLocations.forEach((loc) => {
        const position = new google.maps.LatLng(loc.latitude, loc.longitude)
        const marker = new google.maps.Marker({
          position,
          map: this.map
        })

        // render markers with bounded viewpoints
        this.markers.push(marker)
        this.map.fitBounds(this.bounds.extend(position))
      })
    }, // setMarkers
    // gets query from search bar and geocode the location inputted
    geocode () {
      var input;
      if (document.getElementById("address-input")) {
        input = document.getElementById('address-input')
      } else {
        input = document.getElementById('address-input2')
      }
      this.autocomplete = new google.maps.places.Autocomplete(input)

      // this code snippet inspired by
      // https://developers.google.com/maps/documentation/javascript/examples/places-autocomplete#try-it-yourself
      this.autocomplete.addListener('place_changed', () => {
        var place = this.autocomplete.getPlace()
        // if place was found through autocompletion,
        // move map center to searched input
        // add to markedLocations
        this.markedLocations.push({
          label: place.name,
          latitude: place.geometry.location.lat(),
          longitude: place.geometry.location.lng()
        })
      })
    } // geocode
  },
  mounted () {
    this.initGoogleMap()
    this.setMarkers()
    this.geocode()
  },
  watch: {
    // check when markedLocations is updated so map can re-render
    markedLocations: function () {
      this.initGoogleMap()
      this.setMarkers()
    }, // markedLocations
    // center the clicked location on map
    locatedPlace: function (newPlace, oldPlace) {
      this.map.setCenter({lat: newPlace.latitude, lng: newPlace.longitude})
    } // locatedPlace
  }
}
</script>

<style scoped>
#map {
  height: 100%;
  width: 100%;
}
</style>
