<template>
  <v-btn
      :color="color"
      :loading="loading"
      :elevation="elevation">
    {{ text }}
  </v-btn>
</template>

<script>
export default {
  name: "Button",
  props: {
    text: {
      type: String,
      required: true
    },
    color: {
      type: String,
      default: "primary"
    },
    loading: {
      type: Boolean,
      default: false
    },
    elevation: {
      type: Number,
      default: 3
    }
  }
}
</script>

<style scoped>

</style>
