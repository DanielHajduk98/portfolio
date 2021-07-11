<template>
  <Layout>
    <div class="page">
      <div class="page__container">
        <transition name="resize" v-on:after-enter="afterEnter">
          <div v-if="reveal" class="reveal"></div>
        </transition>

        <div v-if="showPiano" class="piano-wrapper">
          <div class="piano">
            <button
              :ref="`btn${index === 10 ? 0 : index}`"
              @click="play(index === 10 ? 0 : index)"
              class="piano__note"
              v-for="index in 10"
              :key="index"
            >
              <span>{{ `btn${index === 10 ? 0 : index}` }}</span>
            </button>
          </div>
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
    pressNote(note) {
      const btnRef = this.$refs[`btn${note}`][0];

      btnRef.classList.add("active");
      btnRef.click();
      setTimeout(() => {
        btnRef.classList.remove("active");
      }, 100);
    },

    play(note) {
      console.log(note);
    },

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

    let self = this;
    window.onkeyup = function (event) {
      const key = event.key;

      if (self.showPiano && !isNaN(key) && key !== " ") {
        self.pressNote(key);
      }
    };
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
  display: grid;
  grid-template-columns: repeat(10, 1fr);
  grid-gap: 2px;
  width: 30%;
  max-width: 500px;

  &__note {
    background: #fff;
    height: 125px;
    border-radius: 0 0 15px 15px;
    box-shadow: 2px 2px 4px gray;
    border: none;
    display: flex;
    justify-content: flex-end;
    flex-direction: column;

    & > span {
      font-size: 1rem;
      text-align: center;
      margin-bottom: 10px;
    }

    &.active,
    &:active {
      box-shadow: 2px 2px 4px gray, 1px 0px rgba(0, 0, 0, 0.1) inset,
        -1px 0px rgba(0, 0, 0, 0.1) inset;
      background: linear-gradient(to bottom, #fff 0%, #eee 100%);
    }
  }
}
</style>
