<template>
  <div id="app">
    <div class="page-container">
      <md-app>
        <md-app-toolbar class="md-primary">
          <span class="main-title">Logo</span>
          <md-button
            class="md-icon-button"
            v-show="isMapPage"
            to="/"
          >
            <md-icon>list_alt</md-icon>
          </md-button>
        </md-app-toolbar>

        <md-app-content>
          <router-view/>
        </md-app-content>
      </md-app>
    </div>
  </div>
</template>

<script>
export default {
  name: 'app',
  data: _ => ({
    menuVisible: false
  }),
  computed: {
    isMapPage () {
      return this.$route.name === 'map'
    }
  }
}
</script>

<style lang="scss">
  * {
    box-sizing: border-box;
  }

  #app, .page-container {
    height: 100vh;
  }

  .md-app {
    height: 100%;
  }

  .md-drawer {
    width: 230px;
    max-width: calc(100vw - 125px);
  }

  .main-title {
    flex: 1;
    text-transform: uppercase;
    font-weight: bold;
    font-size: 20px;
    letter-spacing: .6px;
  }

  @media (max-width: 600px) {
    .main-title {
      text-align: center;
    }
  }
</style>
