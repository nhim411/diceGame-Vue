<template>
  <div class="wrapper-popup" :class="getClassPopup">
    <div class="rule">
      <h3>Luật chơi</h3>
      <p>
        Chọn <b>NER GAME</b> để bắt đầu trò chơi. Ở mỗi lượt <b>ROLL DICE</b> điểm đổ được sẽ tính
        trong điểm tạm thời (current), người chơi có thể chọn <b>HOLD</b> để cộng vào điểm chính
        hoặc chọn <b>ROLL DICE</b> để tiếp tục để xúc xắc để có điểm cao hơn. Nếu người chơi đổ phải
        điểm 1 ở ít nhất 1 con xúc sắc sẽ mất lượt chơi. Người đạt điểm <b>FINAL SCORE</b> sớm nhất
        sẽ giành chiến thắng.
      </p>
      <button v-on:click="confirm" class="confirm">Đã hiểu</button>
    </div>
  </div>
</template>

<script>
export default {
  name: 'popup-rule',
  props: {
    isOpenPopup: {
      type: Boolean,
      default: false,
    },
  },
  methods: {
    confirm() {
      this.$emit('handleConfirm');
    },
  },
  computed: {
    getClassPopup() {
      return { 'open-popup': this.isOpenPopup };
    },
  },
};
</script>

<style>
.wrapper-popup {
  position: fixed;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  background-color: rgba(0, 0, 0, 0.4);
  opacity: 0;
  visibility: hidden;
  transition: all 0.3s ease;
}
.wrapper-popup.open-popup {
  opacity: 1;
  visibility: visible;
}
.rule {
  width: 350px;
  padding: 20px;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%) scale(1.2);
  background-color: #fff;
  position: absolute;
  font-family: Arial, Helvetica, sans-serif;
  transition: all 0.3s ease;
}
.open-popup .rule {
  transform: translate(-50%, -50%) scale(1);
}
/* scale -> class */
.rule h3 {
  margin-bottom: 10px;
}
.rule .confirm {
  cursor: pointer;
  margin-top: 20px;
  padding: 8px 15px;
  border: 2px solid #333;
  background-color: #fff;
  transition: all 0.3s ease;
}
.rule .confirm:hover {
  color: #fff;
  background-color: #333;
}
</style>
