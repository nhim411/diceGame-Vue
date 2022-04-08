<template>
  <div id="app">
    <div class="wrapper clearfix">
      <Players
        v-bind:scoresPlayer="scoresPlayer"
        v-bind:currentScore="currentScore"
        v-bind:activePlayer="activePlayer"
        v-bind:isWinner="isWinner"
      />
      <Controls
        v-on:handleNewGame="handleNewGame"
        v-on:handleRollDice="handleRollDice"
        v-on:handleHoldScore="handleHoldScore"
        v-bind:finalScore="finalScore"
        v-bind:isPlaying="isPlaying"
        v-on:handleInputScore="handleInputScore"
      />
      <Dices v-bind:dices="dices" />
      <PopupRule :isOpenPopup="isOpenPopup" v-on:handleConfirm="handleConfirm" />
      <Toast ref="ToastComp" />
    </div>
  </div>
</template>

<script>
import Players from './components/Players.vue';
import Controls from './components/Controls.vue';
import Dices from './components/Dices.vue';
import PopupRule from './components/PopupRule.vue';
import Toast from './components/Toasts.vue';
export default {
  name: 'app',
  data() {
    return {
      scoresPlayer: [0, 0],
      currentScore: 0,
      activePlayer: 0,
      isOpenPopup: false,
      isPlaying: false,
      dices: [6, 6],
      finalScore: 10,
    };
  },
  methods: {
    switchPlayer() {
      this.activePlayer = this.activePlayer === 1 ? 0 : 1;
    },
    handleNewGame() {
      this.isOpenPopup = true;
    },
    // Start new game
    handleConfirm() {
      this.isPlaying = true;
      this.isOpenPopup = false;
      this.scoresPlayer = [0, 0];
      this.currentScore = 0;
      this.activePlayer = Math.floor(Math.random() * 2); //random first player
      this.dices = [1, 1];
    },
    handleRollDice() {
      if (this.isPlaying) {
        let dice1 = Math.floor(Math.random() * 6) + 1;
        let dice2 = Math.floor(Math.random() * 6) + 1;
        this.dices = [dice1, dice2];
        if (dice1 === 1 || dice2 === 1) {
          if (!this.isWinner) this.switchPlayer();
          this.$refs.ToastComp.makeToast(
            `Bạn đã đổ ra xúc sắc 1, đổi lượt chơi sang Player ${
              this.activePlayer === 0 ? '1' : '2'
            }`,
          );
          this.currentScore = 0;
        } else {
          this.currentScore = dice1 + dice2;
        }
      } else {
        // call children method to show toast form parent comp using ref
        this.$refs.ToastComp.makeToast('Vui lòng chọn bắt đầu trò chơi!');
      }
    },
    handleHoldScore() {
      if (this.isPlaying) {
        if (this.currentScore !== 0) {
          let newScoresPlayer = [...this.scoresPlayer];
          let { activePlayer, currentScore } = this;
          let oldScore = newScoresPlayer[activePlayer];
          newScoresPlayer[activePlayer] = currentScore + oldScore;
          this.scoresPlayer = newScoresPlayer;
          this.currentScore = 0;
          if (!this.isWinner) this.switchPlayer();
        } else this.$refs.ToastComp.makeToast('Bạn chưa đổ xúc xắc');
      } else {
        this.$refs.ToastComp.makeToast('Vui lòng chọn bắt đầu trò chơi!');
      }
    },
    handleInputScore({ input }) {
      this.finalScore = input;
    },
  },
  computed: {
    isWinner() {
      let { scoresPlayer, finalScore } = this;
      console.log(scoresPlayer, finalScore);
      if (scoresPlayer[0] >= finalScore || scoresPlayer[1] >= finalScore) {
        // eslint-disable-next-line vue/no-side-effects-in-computed-properties
        this.isPlaying = false;
        return true;
      } else {
        return false;
      }
    },
  },
  components: { Players, Controls, Dices, PopupRule, Toast },
};
</script>

<style>
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

.clearfix::after {
  content: '';
  display: table;
  clear: both;
}

body {
  background-image: linear-gradient(rgb(88 161 230 / 40%), rgb(116 131 209 / 40%)),
    url('https://i.pinimg.com/originals/ed/b2/31/edb231165ee8e1d9f8536395041bc389.gif');
  background-size: cover;
  background-position: center;
  font-family: Lato;
  font-weight: 300;
  position: relative;
  height: 100vh;
  color: #555;
}

.wrapper {
  width: 1000px;
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  background-color: #fff;
  box-shadow: 0px 10px 50px rgba(0, 0, 0, 0.3);
  overflow: hidden;
}

.b-toaster-top-right div,
.b-toast,
.toast {
  border: none !important;
}
</style>
