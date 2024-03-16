<template>
  <div class="modal-wrp" @click.self="$emit('closeModal')">
    <div class="modal">
      <div class="close" @click="$emit('closeModal')">&times;</div>
      <h3>Упс!</h3>
      <p>Вы проиграли. В следующий раз получится лучше.</p>
    </div>
  </div>
</template>
  
  <script>
export default {};
</script>
  
  <style lang="scss" scoped>
.modal-wrp {
  background-color: rgba($color: #000000, $alpha: 0.7);
  position: fixed;
  width: 100%;
  height: 100%;
  top: 0;
  left: 0;
  display: flex;
  align-items: center;
  justify-content: center;
  .modal {
    border-radius: 10px;
    padding: 50px;
    background-color: #fff;
    position: relative;
    h3 {
      font-weight: 400;
      font-size: 34px;
      color: #000;
      margin-top: 0;
    }
    p {
      font-weight: 300;
      font-size: 20px;
      color: #191919;
    }
    .close {
      position: absolute;
      top: 20px;
      right: 20px;
      cursor: pointer;
      font-size: 34px;
    }
  }
}
</style>
  