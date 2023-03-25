<template>
  <section class="stopwatches">
    <div class="stopwatches__container container">
      <Stopwatch
        v-for="(stopwatch, index) in stopwatches"
        :key="index"
        :hours="stopwatch.hours"
        :minutes="stopwatch.minutes"
        :seconds="stopwatch.seconds"
      />
      <NewStopwatch @click="addNewStopwatch"/>
    </div>
  </section>
</template>

<script>
import Stopwatch from './Stopwatch/Stopwatch.vue'
import NewStopwatch from './Stopwatch/NewStopwatch.vue'
import { ref } from 'vue';

export default {
  name: 'Stopwatches',
  components: {
    Stopwatch,
    NewStopwatch
  },
  setup() {
    const stopwatches = ref([
      {hours: 0, minutes: 2, seconds: 40},
      {hours: 0, minutes: 25, seconds: 32},
      {hours: 1, minutes: 30, seconds: 15},
      {hours: 1, minutes: 20, seconds: 33},
    ]);

    const addNewStopwatch = () => {
      stopwatches.value.push({hours: 0, minutes: 0, seconds: 0});
    }

    return {
      stopwatches,
      addNewStopwatch
    }
  }
}
</script>

<style scoped lang="scss">
  .stopwatches {
    display: flex;
    justify-content: center;

    &__container {
      display: grid;
      grid-template-columns: repeat(3, 225px);
      row-gap: 45px;
      column-gap: 50px;
      margin: 0 auto;

      @media (max-width: 1023px) {
        grid-template-columns: repeat(2, 225px);
      }

      @media (max-width: 767px) {
        grid-template-columns: 225px;
      }
    }
  }
</style>
