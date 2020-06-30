<template>
  <div class="google-map__wrapper">
    <md-button
      @click="saveLocation"
      class="btn-save-marker md-primary md-raised"
    >
      Post
    </md-button>
    <GmapMap
      class="google-map"
      :disableDefaultUI="false"
      :center="{ lat: lat, lng: lng }"
      :zoom="12"
      map-type-id="terrain"
      @click="mapClicked"
    >
      <gmap-autocomplete>
        @place_changed="getPlace"
      </gmap-autocomplete>
      <GmapMarker
        :key="index"
        :icon="m.position.id ? null : customIcon"
        v-for="(m, index) in markers.concat(newPointPosition)"
        :position="m.position"
        :clickable="false"
        :draggable="false"
        @click="center=m.position"
      />
    </GmapMap>
  </div>
</template>

<script>
export default {
  name: 'google-map',
  props: {
    lat: {
      type: Number,
      default: 53
    },
    lng: {
      type: Number,
      default: 9
    }
  },
  data: _ => ({
    message: '',
    markers: [],
    customIcon: {
      url: 'https://cdn3.iconfinder.com/data/icons/gray-toolbar-4/512/map_marker-512.png',
      size: { width: 38, height: 42, f: 'px', b: 'px' },
      scaledSize: { width: 38, height: 42, f: 'px', b: 'px' }
    },
    newPointPosition: []
  }),
  methods: {
    mapClicked (point) {
      this.newPointPosition.splice(0, 1, {
        position: {
          lat: point.latLng.lat(),
          lng: point.latLng.lng()
        }
      })
    },
    saveLocation () {
      this.$http.post('https://test-app-map.herokuapp.com/points', {
        point: {
          lat: this.newPointPosition[0].position.lat,
          lng: this.newPointPosition[0].position.lng
        }
      }).then(() => {
        this.$router.push({ name: 'home', params: { status: 'Success' } })
      }, response => {
        this.message = 'Fail'
      })
    }
  },
  mounted () {
    this.$http.get('https://test-app-map.herokuapp.com/points').then(response => {
      response.body.forEach(el => {
        this.markers.push({
          position: {
            id: el.id,
            lat: el.lat,
            lng: el.lng
          }
        })
      })
    }, response => {
      this.message = 'Fail'
    })
  }
}
</script>

<style scoped lang="scss">
  .google-map__wrapper {
    position: relative;
  }

  .google-map {
    width: 100%;
    height: calc(100vh - 48px);
  }

  .btn-save-marker {
    position: absolute;
    bottom: 0;
    z-index: 1000;
  }
</style>
