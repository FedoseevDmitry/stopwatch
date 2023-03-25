<template>
  <div class="stopwatch">
    <img class="stopwatch__img" src="@/assets/images/plus.svg" alt="Плюс" height="20" width="20">
  </div>
</template>

<script>
export default {
  name: 'NewStopwatch',
}
</script>

<style scoped lang="scss">
  .stopwatch {
    display: flex;
    justify-content: center;
    align-items: center;
    background-color: $greyColor;
    height: 120px;

    &__img {
      cursor: pointer;
      height: 20px;
      width: 20px;
      transition: all ease-in-out .3s;

      &:hover {
        filter: brightness(200%);
      }
    }
  }
</style>