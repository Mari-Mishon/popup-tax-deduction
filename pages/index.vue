
<template>
  <div>
    <button class="btn" @click="showPopup">Налоговый вычет</button>
    <Popup v-if="popupVisible" @closePopup="closePopup" />
  </div>
</template>

<script>
import Popup from "@/components/Popup.vue";

export default {
  components: {
    Popup,
  },
  data() {
    return {
      popupVisible: false,
    };
  },
  methods: {
    showPopup() {
      this.popupVisible = true;
    },
    closePopup() {
      this.popupVisible = false;
    },
  },
};
</script>

<style lang="scss">
@font-face {
  font-family: "LabGrotesque";
  src: 
    url("@/assets/fonts/LabGrotesque-Regular.woff") format("woff"),
    url("@/assets/fonts/LabGrotesque-Regular.woff2") format("woff2");
  font-weight: 400;
  font-style: normal;
  font-stretch: normal;
  font-variant: normal;
}

@font-face {
  font-family: "LabGrotesque";
  src: 
    url("@/assets/fonts/LabGrotesque-Medium.woff") format("woff"),
    url("@/assets/fonts/LabGrotesque-Medium.woff2") format("woff2");
  font-weight: 500;
  font-style: normal;
  font-stretch: normal;
  font-variant: normal;
}

.btn {
  display: flex;
  flex-direction: row;
  align-items: center;
  padding: 16px 32px;
  position: absolute;
  width: 200px;
  height: 56px;
  left: 621px;
  top: 372px;

  font-family: LabGrotesque, sans-serif;
  font-style: normal;
  font-weight: 500;
  font-size: 16px;
  line-height: 24px;
  background: none;
  border: 1px solid #ffffff;
  box-sizing: border-box;
  filter: drop-shadow(0px 0px 44px #cfdae7);
  border-radius: 6px;
}

.btn:hover {
  background: white;
  color: black;
  filter: none;
  cursor: url("@/assets/img/pointer.svg"), pointer;
}
</style>