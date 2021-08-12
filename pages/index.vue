<template>
  <div>
    <Popup v-if="popupVisible" @closePopup="closePopup" />
    <button class="btn" @click="showPopup">Налоговый вычет</button>
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
  src: url("@/assets/fonts/LabGrotesque-Regular.woff") format("woff"),
    url("@/assets/fonts/LabGrotesque-Regular.woff2") format("woff2");
  font-weight: 400;
  font-style: normal;
  font-stretch: normal;
  font-variant: normal;
}

@font-face {
  font-family: "LabGrotesque";
  src: url("@/assets/fonts/LabGrotesque-Medium.woff") format("woff"),
    url("@/assets/fonts/LabGrotesque-Medium.woff2") format("woff2");
  font-weight: 500;
  font-style: normal;
  font-stretch: normal;
  font-variant: normal;
}

.btn {
 position: absolute;
    top: 0;
    right: 0;
    bottom: 0;
    left: 0;
    margin: auto;
  padding: 16px 32px;
  width: 200px;
  height: 56px;
  color: white;
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
      z-index:-1;

}

.btn:hover {
  background: white;
  color: black;
  filter: none;
  cursor: url("@/assets/img/pointer.svg"), pointer;
}
</style>