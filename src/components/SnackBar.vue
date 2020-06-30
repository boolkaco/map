<template>
  <md-snackbar md-position="left" :md-duration="4000" :md-active.sync="show" md-persistent>
    <span>
      {{ message }}
    </span>
  </md-snackbar>
</template>

<script>
export default {
  name: 'snack-bar',
  props: {
    show: Boolean,
    message: String
  }
}
</script>

<style scoped lang="scss">
  //
</style>
