<template>
  <div class="popup">
    <div class="popup__content">
      <span>Popup logics</span>
      <span @click="closePopup">Close X</span>
    </div>
  </div>
</template>

<script>
export default {
  methods: {
    closePopup() {
      this.$emit('closePopup')
    }
  }
};
</script>
<style lang="scss" scoped>
.popup {
  top: 0;
  bottom: 0;
  right: 0;
  left: 0;
  background-color: gray;
  position: fixed;
  display: flex;
}

.popup__content {
  margin: auto;
  background-color: white;
  border-radius: 30px;
  min-height: 476px;
  min-width: 552px;
  padding: 20px;
}
</style>