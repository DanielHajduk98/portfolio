<template>
  <Layout>
    <div class="page">
      <div class="page__container">
        <transition name="resize" v-on:after-enter="afterEnter">
          <div v-if="reveal" class="reveal"></div>
        </transition>
        <div v-if="showPiano" class="piano-wrapper">
          <div class="piano">piano</div>
        </div>
        <div v-else class="text">
          <h1 class="text__title">🎉</h1>
          <h4 class="text__subtitle">Made to waste your time!</h4>
        </div>
      </div>
    </div>
  </Layout>
</template>

<script>
export default {
  data() {
    return {
      showPiano: false,
      reveal: false,
    };
  },

  metaInfo: {
    title: "🎉",
  },

  methods: {
    swapElements() {
      setTimeout(() => {
        this.reveal = true;
      }, 1000);
    },

    afterEnter() {
      this.reveal = false;
      this.showPiano = true;
    },
  },

  mounted() {
    this.swapElements();
  },
};
</script>

<style lang="scss" scoped>
.page {
  background: $background;
  height: 100vh;

  &__container {
    @extend .container, .center;
    position: relative;
    margin: auto;
    padding: 0;

    height: 100%;
    flex-direction: column;
    display: flex;
    align-content: center;
    justify-content: center;
  }
}

.page-content {
  display: flex;
  justify-content: center;
}

.reveal {
  background: $background;
  position: absolute;
  left: 0;
  bottom: 0;
  width: 100%;
  height: 100%;
}

.resize-enter-active,
.resize-leave-active {
  transition: width 0.5s linear;
}

.resize-enter,
.resize-leave-to {
  width: 0;
}

.text {
  text-align: center;

  &__title {
    font-size: 4rem;
    color: #252525;
    text-shadow: 2px 0 3.125rem gray;
  }

  &__subtitle {
    font-size: 1.5rem;
    color: #252525;
    text-shadow: 2px 0 1.1rem gray;
  }
}

.piano-wrapper {
  @extend .container, .center;
}

.piano {
  text-align: center;
}
</style>
