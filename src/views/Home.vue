<template>
  <div class="md-layout md-gutter home-wrapper">
    <md-card class="main-card md-layout-item md-large-size-50 md-medium-size-70 md-small-size-70 md-xsmall-size-100">
      <md-card-header>
        <div class="md-title">Locations</div>
      </md-card-header>

      <md-card-content>
        <div class="md-layout md-gutter">
          <div class="md-layout-item md-large-size-100 md-medium-size-100 md-small-size-100">
            <div class="autocomplete-label">
              Property address
            </div>
          </div>
          <div class="md-layout-item md-large-size-100 md-medium-size-100 md-small-size-100">
            <gmap-autocomplete
              class="map-input--autocomplete"
              @place_changed="processLocationChanged"
            >
              <template v-slot:input="slotProps">
                <input
                  ref="input"
                  type="text"
                  placeholder="slotted plain old textbox"
                  v-on:listeners="slotProps.listeners"
                  v-on:attrs="slotProps.attrs"
                />
              </template>
            </gmap-autocomplete>
          </div>
        </div>
      </md-card-content>

      <md-progress-bar md-mode="indeterminate" v-if="sending"/>

      <md-card-actions>
        <md-button
          @click="submit"
          class="md-primary md-raised"
          :disabled="sending || disableSubmit"
        >
          Post
        </md-button>
      </md-card-actions>
    </md-card>
  </div>
</template>

<script>
// @ is an alias to /src
import isEmpty from 'lodash.isempty'

export default {
  name: 'home',
  props: {
    status: String
  },
  data: _ => ({
    snackbarMessage: '',
    sending: false,
    place: {}
  }),
  methods: {
    processLocationChanged (place) {
      this.place = {
        lat: place.geometry.location.lat(),
        lng: place.geometry.location.lng()
      }
    },
    submit () {
      this.$router.push({ name: 'map', params: { lat: this.place.lat, lng: this.place.lng } })
    }
  },
  computed: {
    disableSubmit () {
      return isEmpty(this.place)
    }
  },
  mounted () {
    if (this.status) {
      this.snackbarMessage = this.status
    }
  }
}
</script>

<style lang="scss">
  .map-input--autocomplete {
    border-top: 0;
    border-left: 0;
    border-right: 0;
    border-bottom: 1px solid #eee;
    font-size: 15px;
    box-shadow: none;
    padding: 8px 8px;
    width: 100%;

    &:focus {
      border-color: #448aff;
    }
  }

  .autocomplete-label {
    font-size: 13px;
    font-weight: bold;
    padding: 4px 8px 4px;
  }
</style>

<style lang="scss" scoped>
  .md-progress-bar {
    position: absolute;
    top: 0;
    right: 0;
    left: 0;
  }

  @media (min-width: 601px) {
    .home-wrapper {
      justify-content: center;
      align-items: center;
    }
  }

  .main-card {
    margin-top: 16px;
  }
</style>
