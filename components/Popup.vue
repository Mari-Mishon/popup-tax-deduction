<template>
  <div class="popup">
    <div class="popup__content">
      <button class="popup__close-btn" @click="closePopup"></button>
      <TaxForm />
    </div>
  </div>
</template>

<script>
import TaxForm from "@/components/TaxForm.vue";

export default {
  components: { TaxForm },
  methods: {
    closePopup() {
      this.$emit("closePopup");
    },
  },
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


&__content {
position:relative;
  margin: auto;
  background-color: white;
  border-radius: 30px;
  max-width: 552px;
  padding: 32px;
}

&__close-btn {
    width:18px;
    height:18px;
    color:red;
    border: none;
    background:url("@/assets/img/close.svg");
    cursor: url("@/assets/img/pointer.svg"), pointer;
    position: absolute;
    right: 27px;
    top: 27px;
  }
}
</style>