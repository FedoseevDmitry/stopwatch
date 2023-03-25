<template>
  <section class="stopwatches">
    <Stopwatch/>
  </section>
</template>

<script>
import Stopwatch from './Stopwatch/Stopwatch.vue'

export default {
  name: 'Stopwatches',
  components: {
    Stopwatch
  }
}
</script>

<style scoped lang="scss">
  .stopwatches {
    display: grid;
    grid-template-columns: repeat(3, 225px);
  }
</style>
