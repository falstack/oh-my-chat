<style lang="scss">
.loading-msg {
  $primary-color: #fff;

  .dot {
    width: 0.5rem;
    height: 0.5rem;
    border-radius: 0.5rem;
    display: inline-block;
    background-color: $primary-color;
    &:nth-last-child(1) {
      margin-left: 0.3rem;
      animation: loading 0.6s 0.3s linear infinite;
    }
    &:nth-last-child(2) {
      margin-left: 0.3rem;
      animation: loading 0.6s 0.2s linear infinite;
    }
    &:nth-last-child(3) {
      animation: loading 0.6s 0.1s linear infinite;
    }
  }

  @keyframes loading {
    0% {
      transform: translate(0, 0);
      background-color: lighten($primary-color, 10%);
    }

    25% {
      transform: translate(0, -3px);
    }
    50% {
      transform: translate(0, 0px);
      background-color: lighten($primary-color, 10%);
    }
    75% {
      transform: translate(0, 3px);
    }
    100% {
      transform: translate(0, 0px);
    }
  }

  @-webkit-keyframes loading {
    0% {
      transform: translate(0, 0);
      background-color: lighten($primary-color, 10%);
    }

    25% {
      transform: translate(0, -3px);
    }
    50% {
      transform: translate(0, 0px);
      background-color: lighten($primary-color, 10%);
    }
    75% {
      transform: translate(0, 3px);
    }
    100% {
      transform: translate(0, 0px);
    }
  }
}
</style>

<template>
  <div class="loading-msg">
    <i class="dot" />
    <i class="dot" />
    <i class="dot" />
  </div>
</template>

<script>
export default {
  name: 'LoadingMsg'
}
</script>
