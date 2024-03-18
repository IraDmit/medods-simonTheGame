<template>
  <div id="app" class="container">
    <h1>Simon The Game</h1>
    <div class="main">
      <div class="game-wrp" :class="{ disabled: disabledUI }">
        <div
          class="block"
          v-for="(block, idx) in blocksList"
          :key="block"
          :class="[block, { active: idx === activeColor }]"
          @click="checkPlayerInput(idx)"
        ></div>
      </div>
      <audio ref="audioPlayer" autoplay>
        <source :src="audioSrc" type="audio/ogg" />
      </audio>
      <div class="settings">
        <div class="btn" @click="startGame" :class="{ disabled: disabledUI }">
          Начать игру
        </div>
        <p>Раунд: {{ round }}</p>
        <div class="lvl-stng">
          <span>Выберите уровень сложности:</span>
          <div class="lvl-wrp">
            <div class="lvl ease" @click="delay = 1500">Легкий</div>
            <div class="lvl normal" @click="delay = 1000">Нормальный</div>
            <div class="lvl hard" @click="delay = 400">Сложный</div>
          </div>
        </div>
      </div>
    </div>
    <modalFail v-if="isOpen" @closeModal="closeModal" />
  </div>
</template>

<script>
import modalFail from "./components/modals/modalFail.vue";

export default {
  components: { modalFail },
  data() {
    return {
      delay: 1500,
      round: 0,
      sequence: [],
      // при заливании на github pages с помощью gh-pages звуки не заливались, поэтому сделала ссылкой
      audioFiles: {
        0: "http://www.kellyking.me/projects/simon/sounds/1.ogg",
        1: "http://www.kellyking.me/projects/simon/sounds/2.ogg",
        2: "http://www.kellyking.me/projects/simon/sounds/3.ogg",
        3: "http://www.kellyking.me/projects/simon/sounds/4.ogg",
      },
      disabledUI: false,
      activeColor: null,
      humanStack: [],
      blocksList: ["blue", "red", "yellow", "green"],
      countClick: 0,
      isOpen: false,
      audioSrc: "",
    };
  },
  mounted() {
    this.test = document.querySelectorAll(".block");
  },
  watch: {
    countClick(n) {
      if (n >= this.sequence.length) {
        this.startRound();
      }
    },
  },
  methods: {
    // start  game, generateQueue, checkPlayerInput, resetGame, displaySequence
    startGame() {
      this.startRound();
    },
    startRound() {
      this.round++;
      this.countClick = 0;
      this.generateQueue();
      this.displaySequence();
    },
    random() {
      return Math.floor(Math.random() * this.blocksList.length);
    },
    generateQueue() {
      this.sequence.push(this.random());
    },
    // 1е решение
    // async displaySequence() {
    // let i = 0;
    // const maxLentgh = this.sequence.length;
    // const interval = setInterval(() => {
    //   if (i > maxLentgh) {
    //     clearInterval(interval);
    //   } else {
    //     this.activeColor = this.sequence[i];
    //     i++;
    //     setTimeout(() => {
    //       this.activeColor = null;
    //     }, this.delay);
    //   }
    // }, this.delay);
    // },
    // 2е решение
    async displaySequence() {
      this.disabledUI = true;
      for (let i = 0; i < this.sequence.length; i++) {
        this.activeColor = this.sequence[i];
        this.playAudio(this.activeColor);
        await new Promise((resolve) => setTimeout(resolve, this.delay));
        this.activeColor = null;
        await new Promise((resolve) => setTimeout(resolve, this.delay));
      }
      this.disabledUI = false;
    },

    checkPlayerInput(colorIdx) {
      if (!this.sequence.length) return;
      this.playAudio(colorIdx);
      if (this.sequence[this.countClick] !== colorIdx) {
        this.resetGame();
      } else {
        this.countClick++;
      }
    },
    playAudio(index) {
      const audioPlayer = this.$refs.audioPlayer;
      audioPlayer.src = this.audioFiles[index];
      audioPlayer.play();
    },
    resetGame() {
      this.countClick = 0;
      this.round = 0;
      this.sequence = [];
      this.isOpen = true;
    },
    closeModal() {
      this.isOpen = false;
    },
  },
};
</script>

<style lang="scss" scoped>
@import url("https://fonts.googleapis.com/css2?family=Montserrat:ital,wght@0,100..900;1,100..900&display=swap");
#app {
  font-family: "Montserrat";
  display: flex;
  flex-direction: column;
  height: 100vh;
  .main {
    height: 100%;
    display: grid;
    grid-template-columns: 0.7fr 0.3fr;
    grid-gap: 40px;
    @media (max-width: 820px) {
      grid-template-columns: 1fr;
    }
    .game-wrp {
      height: 50vh;
      display: grid;
      grid-template-columns: 1fr 1fr;
      grid-gap: 20px;
      .block {
        width: 100%;
        cursor: pointer;
        opacity: 0.5;
        transition: 0.3s;
        &.yellow {
          background-color: yellow;
        }
        &.blue {
          background-color: blue;
        }
        &.red {
          background-color: red;
        }
        &.green {
          background-color: green;
        }
        &.active,
        &:hover {
          opacity: 1;
        }
      }
    }
    .settings {
      .lvl-stng {
        .lvl-wrp {
          display: flex;
          flex-wrap: wrap;
          grid-gap: 10px;
          margin-top: 10px;
          .lvl {
            cursor: pointer;
            border-radius: 4px;
            padding: 5px 10px;
            &.ease {
              background-color: #59e62e;
            }
            &.normal {
              background-color: #f0ed3e;
            }
            &.hard {
              background-color: #e62e43;
            }
          }
        }
      }
    }
  }
}
.btn {
  padding: 10px 20px;
  background-color: #f0ed3e;
  border-radius: 5px;
  text-align: center;
  font-weight: 600;
  transition: 0.3s;
  cursor: pointer;
  &.disabled {
    background-color: #f4f4f4;
  }
}
.disabled {
  pointer-events: none;
}
</style>
