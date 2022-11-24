<template>
  <v-overlay :value="overlayCopy">
    <v-progress-circular
      indeterminate
      :size="size"
      @click="overlayCopy = !overlayCopy"
    />
  </v-overlay>
</template>

<script>
module.exports = {
  name: 'Loading',
  props: {
    size: {
      type: Number,
      default: 200
    },
    overlay: {
      type: Boolean,
      default: false
    }
  },
  data() {
    return {
      overlayCopy:false,
    }
  },
  watch: {
    'overlay'(to, from) {
      this.overlayCopy = to
    },
  },
  mounted() {
    this.overlayCopy = this.overlay
  },
}
</script>

<style>
</style>
