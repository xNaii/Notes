<template>
  <div class="container-fluid">
    <Notes />
  </div>
</template>

<script>
import Notes from '@/components/Notes.vue'

export default {
  name: 'Home',
  components: {
    Notes
  }
}
</script>

<style scoped>
  .container-fluid {
    width: 99% !important;
  }
</style>