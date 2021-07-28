<template>
  <Layout>
    <div class="page">
      <div class="page__container">
        <transition name="resize" v-on:after-enter="afterEnter">
          <div v-if="reveal" class="reveal"></div>
        </transition>

        <div v-if="showPiano" class="container center piano-wrapper">
          <div class="notes">
            <h1 class="notes__title">{{ notes.title }}</h1>
            <p class="notes__text">{{ notes.notes }}</p>
            <button class="notes__btn" @click="nextNotes()">Next</button>
          </div>

          <div class="piano">
            <button
              :ref="`btn${index === 10 ? 0 : index}`"
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

export default {
  data() {
    return {
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
        yield new Song("Imperial march", "3 3 33 1 53 1 53 7 7 77 8 53 1 53");
        yield new Song(
          "Godfather",
          "3    6 8 7 6   8 6 7 6   4 5 3     3   6 8 7 6    8 6 7 6    3 3 2     2 4 6 7     1 3 4 6 "
        );
        yield new Song("Mc Donald's theme", "123 65");
        yield new Song(
          "Astronomia aka coffin dance",
          "2 2 6 5 4 3  3 4 5  4 3 2  2 4 3 4 3 4   2  2 4 3 4 3 4"
        );
        yield new Song(
          "Happy birthday",
          "1 1 2 1 4 3, 1 1 2 1 5 4, 1 1 8 6 4 3 2, 5 5 2 3 4"
        );
        yield new Song(
          "Tetris",
          "0, 7, 8, 9, 8, 7, 6, 8, 0, 9, 8, 7, 7, 8, 9, 0, 8, 6, 6, 2, 4, 6, 5, 4, 3, 8, 0, 9, 8, 7, 7, 8, 9, 0, 8, 6, 6"
        );
        yield new Song("Force theme", "36 7 8-9-8 3 3-6 7 8 3 8 6 0 9");
        yield new Song(
          "Государственный гимн СССР",
          "8      5 8    5 6 7   3 3 6   5 4 5   1 1   2    2 3 4    4 5 6    7 8 9    5 5 0    9 8 9    5 5 8    7 6 7    3 3 6    5 4 5    1 1 8    7 6 5 0   9   8 7 8 9   5 5   8   7 6 5 6 7    3 3    8   6 7 8    6 7 8   6 8 4    4   0 9 8 9 0   8 8   9   8 7 6 7 8   6 6    8   7   6 5   1   1 5     6   7   8!"
        );
      }
    },

    nextNotes() {
      this.notes = this.notesGen.next().value;
    },

    pressNote(note) {
      const btnRef = this.$refs[`btn${note}`][0];

      btnRef.classList.add("active");
      btnRef.click();
      setTimeout(() => {
        btnRef.classList.remove("active");
      }, 100);
    },

    play(number) {
      this.audio[number].play();
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

    this.nextNotes();

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

  width: 100%;
  height: 100%;
  display: flex;
  justify-content: center;
  flex-direction: column;
  align-items: center;
}

.piano {
  text-align: center;
  display: grid;
  grid-template-columns: repeat(10, 1fr);
  grid-gap: 2px;
  width: 30%;
  max-width: 500px;
  margin-top: 25px;

  &__keys {
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

.notes {
  display: flex;
  align-items: center;
  flex-direction: column;

  @include md {
    max-width: 70vw;
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
</style>
