<template>
  <Layout>
    <div class="page">
      <div class="page__container">
        <transition name="resize" v-on:after-enter="afterEnter">
          <div v-if="reveal" class="reveal"></div>
        </transition>

        <div v-if="showPiano" class="piano-wrapper">
          <div class="notes">
            <h1 class="notes__title">{{ notes.title }}</h1>
            <p class="notes__text">{{ notes.notes }}</p>
            <button class="notes__btn" @click="nextSong()">Next</button>
          </div>

          <div class="piano">
            <button
              ref="btn"
              @click="play(index === 10 ? 0 : index)"
              class="piano__keys"
              v-for="index in 10"
              :key="index"
            >
              <span>{{ index === 10 ? 0 : index }}</span>
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
import { Howl } from "howler/dist/howler.core.min";
import E5 from "@/assets/sounds/piano/e5.mp3";
import C4 from "@/assets/sounds/piano/c4.mp3";
import D4 from "@/assets/sounds/piano/d4.mp3";
import E4 from "@/assets/sounds/piano/e4.mp3";
import F4 from "@/assets/sounds/piano/f4.mp3";
import G4 from "@/assets/sounds/piano/g4.mp3";
import A4 from "@/assets/sounds/piano/a5.mp3";
import B4 from "@/assets/sounds/piano/b5.mp3";
import C5 from "@/assets/sounds/piano/c5.mp3";
import D5 from "@/assets/sounds/piano/d5.mp3";
import Song from "../assets/objects/song";
import songs from "@/assets/files/songs.js";

export default {
  data() {
    return {
      NUMBER_OF_KEYS: 10,
      notesGen: this.notesGenerator(),
      notes: "",
      showPiano: false,
      reveal: false,
      audio: [
        new Howl({ src: E5 }),
        new Howl({ src: C4 }),
        new Howl({ src: D4 }),
        new Howl({ src: E4 }),
        new Howl({ src: F4 }),
        new Howl({ src: G4 }),
        new Howl({ src: A4 }),
        new Howl({ src: B4 }),
        new Howl({ src: C5 }),
        new Howl({ src: D5 }),
      ],
    };
  },

  metaInfo: {
    title: "🎉",
  },

  methods: {
    *notesGenerator() {
      while (true) {
        for (let song of songs) {
          yield new Song(song.title, song.notes);
        }
      }
    },

    nextSong() {
      this.notes = this.notesGen.next().value;
    },

    pressNote(note) {
      const btnRef = this.$refs["btn"][note];

      btnRef.classList.add("active");
      btnRef.click();
      setTimeout(() => {
        btnRef.classList.remove("active");
      }, 100);
    },

    play(number) {
      this.audio[number].play();
    },

    async swapElements() {
      await setTimeout(() => {
        this.reveal = true;
      }, 1000);
    },

    afterEnter() {
      this.reveal = false;
      this.showPiano = true;
    },

    addOnKeyPressWatcher() {
      window.onkeydown = this.keyPressWatcher;
    },

    keyPressWatcher(event) {
      const key = event.key;

      if (this.showPiano) {
        if (key !== " " && !isNaN(key)) {
          this.pressNote(key);
        } else if (key === "Enter") {
          this.nextSong();
        }
      }
    },
  },

  mounted() {
    this.swapElements();
    this.nextSong();
    this.addOnKeyPressWatcher();
  },
};
</script>

<style lang="scss" scoped>
.page {
  background: $background;
  height: 100vh;

  &__container {
    @extend .container, .center, .box-sizing-border-box;

    position: relative;
    margin: auto;

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
  @extend .container, .box-sizing-border-box;

  height: 100%;
  display: grid;
  grid-template-rows: repeat(2, 1fr);
  grid-template-columns: 1fr;
  justify-content: center;
  align-items: center;

  width: 100%;

  @include sm {
    grid-gap: 25px;
    display: flex;
    flex-direction: column;
  }

  @include md {
    width: 70vw;
  }

  @include lg {
    width: 45vw;
  }
}

.notes {
  display: flex;
  align-items: center;
  flex-direction: column;
  align-self: end;

  @include sm {
    align-self: center;
  }

  &__text {
    white-space: pre-wrap;
  }

  &__title {
    font-size: 2rem;
    font-weight: bold;
    text-align: center;
  }

  &__btn {
    & > img {
      max-width: 64px;
      width: 100%;
      height: auto;
    }
  }
}

.piano {
  text-align: center;
  display: grid;
  width: 100%;

  grid-template-columns: repeat(3, 1fr);
  grid-template-rows: repeat(4, 1fr);
  grid-gap: 5px;

  @include sm {
    align-self: start;
    grid-gap: 2px;
    grid-template-rows: 1fr;
    grid-template-columns: repeat(10, 1fr);
  }

  &__keys {
    background: #fff;
    box-shadow: 2px 2px 4px gray;
    border: none;
    display: flex;
    flex-direction: column;

    height: 8vh;
    border-radius: 15px;
    justify-content: center;

    &:is(:last-child) {
      grid-column: 1/4;
    }

    @include sm {
      height: auto;
      border-radius: 0 0 15px 15px;
      justify-content: flex-end;

      // Responsible for aspect ratio
      &::before {
        content: "";
        display: inline-block;
        width: 1px;
        padding-bottom: calc(100% / (1 / 1.7));
      }

      &:is(:last-child) {
        grid-column: auto;
      }
    }

    & > span {
      font-size: 1rem;
      text-align: center;

      @include sm {
        margin-bottom: 10px;
      }
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
