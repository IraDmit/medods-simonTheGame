<template>
  <div id="app" class="container">
    <h1>Simon The Game</h1>
    <div class="main">
      <div class="game-wrp">
        <div
          class="block"
          v-for="(block, idx) in blocksList"
          :key="block"
          :class="[block, { active: idx === activeColor }]"
          @click="checkPlayerInput(idx)"
        ></div>
      </div>
      <div class="settings">
        <h2 @click="startGame">Начать игру</h2>
        <p>Раунд: {{ round }}</p>
        <div class="lvl-stng">
          <span>Выберите уровень сложности:</span>
          <div class="lvl-wrp">
            <div class="lvl" @click="delay = 1500">Легкий</div>
            <div class="lvl" @click="delay = 1000">Нормальный</div>
            <div class="lvl" @click="delay = 400">Сложный</div>
            {{ delay }}
          </div>
        </div>
      </div>
      {{ sequence }}
      {{ countClick }}
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
      activeColor: null,
      humanStack: [],
      blocksList: ["blue", "red", "yellow", "green"],
      test: null,
      countClick: 0,
      isOpen: false,
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
    async displaySequence() {
      let i = 0;
      const maxLentgh = this.sequence.length;
      const interval = setInterval(() => {
        this.activeColor = this.sequence[i];
        setTimeout(() => {
          i++;
        }, this.delay);
      }, this.delay);
      if (i >= maxLentgh) {
        clearInterval(interval);
      }
      // this.sequence.forEach((el, idx) => {
      //   setTimeout(() => {
      //     this.activeColor = el;
      //     console.log(el);
      //   }, this.delay * (idx - 1));
      // });
    },
    checkPlayerInput(colorIdx) {
      if (this.sequence[this.countClick] !== colorIdx) {
        console.log(111);
        this.resetGame();
      } else {
        this.countClick++;
      }
    },
    resetGame() {
      this.countClick = 0;
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
#app {
  display: flex;
  flex-direction: column;
  height: 100vh;
  .main {
    height: 100%;
    display: grid;
    grid-template-columns: 0.7fr 0.3fr;
    grid-gap: 40px;
    .game-wrp {
      height: 50vh;
      display: grid;
      grid-template-columns: 1fr 1fr;
      grid-gap: 20px;
      .block {
        width: 100%;
        cursor: pointer;
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
        &.active {
          border: 1px solid #fff;
          box-shadow: 0 0 20px #555;
        }
        &:hover {
          border: 1px solid #fff;
          box-shadow: 0 0 20px #555;
        }
      }
    }
    .settings {
      .lvl-stng {
        .lvl-wrp {
          .lvl {
          }
        }
      }
    }
  }
}
</style>
