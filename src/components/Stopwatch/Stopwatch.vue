<template>
  <div
    :class="[{'stopwatch_active': stopwatchRunned}, 'stopwatch']"
  >
    <div class="stopwatch__block">
      <p class="stopwatch__timer">
        <span class="stopwatch__timer" v-if="hours > 0">{{ hours }}:</span>
        <span class="stopwatch__timer" v-if="hours > 0 || minutes > 0">{{ minutes }}:</span>
        <span class="stopwatch__timer">{{ seconds }}</span>
      </p>
    </div>
    <div
      :class="[{'stopwatch__control_active': stopwatchRunned}, 'stopwatch__control']"
    >
      <img
        src="@/assets/images/rectangle.svg"
        alt="Старт"
        width="17"
        height="20"
        v-if="!stopwatchRunned"
        :class="[{'stopwatch__img_active': stopwatchRunned}, 'stopwatch__img', 'stopwatch__img_play']"
        @click="stopwatchStart()"
      >
      <img
        src="@/assets/images/pause.svg"
        alt="Пауза"
        width="17"
        height="20"
        v-if="stopwatchRunned"
        :class="[{'stopwatch__img_active': stopwatchRunned}, 'stopwatch__img', 'stopwatch__img_pause']"
        @click="stopwatchPause()"
      >
      <img
        src="@/assets/images/square.svg" 
        alt="Стоп" 
        width="20" 
        height="20"
        :class="[{'stopwatch__img_active': stopwatchRunned}, 'stopwatch__img', 'stopwatch__img_stop']"
        @click="stopwatchStop()"
      >
    </div>
  </div>
</template>

<script>
import { ref } from 'vue';

export default {
  name: 'Stopwatch',
  props: {
    hours: {
      type: Number,
      default: 0,
    },
    minutes: {
      type: Number,
      default: 0,
    },
    seconds: {
      type: Number,
      default: 0,
    },
  },
  setup(props) {
    let stopwatchRunned = ref(false);
    let hours = ref(props.hours);
    let minutes = ref(props.minutes);
    let seconds = ref(props.seconds);
    let ticker = null;

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
        if ((hours.value > 0 || minutes.value > 0) && seconds.value <= 9) seconds.value = `0${seconds.value}`;
        if (seconds.value  > 59) {
          minutes.value++;
          seconds.value = `0${0}`;
          if (minutes.value > 59) {
            hours.value++;
            minutes.value = 0;
          }
          if (minutes.value <= 9 && hours.value > 0) minutes.value = `0${minutes.value}`;
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
      seconds
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