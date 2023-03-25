<template>
  <div
    class="stopwatch"
    :class="{'stopwatch_active': stopwatchRunned}"
  >
    <div class="stopwatch__block">
      <p class="stopwatch__timer">
        <span class="stopwatch__timer" v-if="hours > 0">{{ hours }}:</span>
        <span class="stopwatch__timer" v-if="hours > 0 || minutes > 0">{{ minutes }}:</span>
        <span class="stopwatch__timer">{{ seconds }}</span>
      </p>
    </div>
    <div
      class="stopwatch__control"
      :class="{'stopwatch__control_active': stopwatchRunned}"
    >
      <img
        class="stopwatch__img stopwatch__img_play"
        src="@/assets/images/rectangle.svg"
        alt="Старт"
        width="17"
        height="20"
        v-if="!stopwatchRunned"
        :class="{'stopwatch__img_active': stopwatchRunned}"
        @click="stopwatchStart()"
      >
      <img
        class="stopwatch__img stopwatch__img_pause"
        src="@/assets/images/pause.svg"
        alt="Пауза"
        width="17"
        height="20"
        v-if="stopwatchRunned"
        :class="{'stopwatch__img_active': stopwatchRunned}"
        @click="stopwatchPause()"
      >
      <img
        class="stopwatch__img stopwatch__img_stop" 
        src="@/assets/images/square.svg" 
        alt="Стоп" 
        width="20" 
        height="20"
        :class="{'stopwatch__img_active': stopwatchRunned}"
        @click="stopwatchStop()"
      >
    </div>
  </div>
</template>

<script>
import { ref } from 'vue';

export default {
  name: 'Stopwatch',
  setup() {
    let stopwatchRunned = ref(false);
    let hours = ref(0);
    let minutes = ref(59);
    let seconds = ref(55);
    let ticker = null;
    let formattedTime = ref('00');

    const stopwatchStart = () => {
      if (stopwatchRunned) {
        tick();
        stopwatchRunned.value = true;
      }
    }

    const stopwatchPause = () => {
      window.clearInterval(ticker);
      stopwatchRunned.value = false;
    }

    const stopwatchStop = () => {
      window.clearInterval(ticker);
      hours.value = 0;
      minutes.value = 0;
      seconds.value = 0;
      stopwatchRunned.value = false;
    }

    const tick = () => {
      ticker = setInterval(() => {
        seconds.value++;
        if (seconds.value  > 59) {
          minutes.value++;
          seconds.value = 0;
        }
        if (minutes.value > 59) {
          hours.value++;
          minutes.value = 0;
        }
      }, 1000)
    }

    return {
      stopwatchRunned,
      stopwatchStart,
      stopwatchPause,
      stopwatchStop,
      hours,
      minutes,
      seconds,
    }
  }
}
</script>

<style scoped lang="scss">
  .stopwatch {
    display: flex;
    flex-direction: column;
    background-color: $greyColor;

    &_active {
      color: #fff;
    }

    &__block {
      display: flex;
      justify-content: center;
      align-items: center;
      padding: 20px 75px;
      height: 60px;
    }

    &__control {
      display: flex;
      justify-content: space-between;
      align-items: center;
      padding: 20px 0;
      height: 60px;
      border-top: 1px solid $lightGreyColor;

      &_active {
        border-top: 1px solid #fff;
      }
    }

    &__img {
      cursor: pointer;

      &_active {
        filter: brightness(200%);
      }

      &_play {
        margin-left: 70px;
      }

      &_pause {
        margin-left: 73px;
        height: 20px;
        width: 10px;
      }

      &_stop {
        margin-right: 70px;
      }
    }
  }
</style>