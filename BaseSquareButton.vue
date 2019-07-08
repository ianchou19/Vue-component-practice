<template>
  <div
    :class="{ clicked: isClick, mode1: isMode1 }"
    class="SquareButton"
    @mousedown="mouseDown"
    @mouseup="mouseUp"
  >{{ buttonText }}</div>
</template>

<script>
export default {
  props: {
    mode: {
      type: String,
      default: "mode1" // mode1 is for light color based, mode2 is for dark color based
    },
    buttonText: {
      type: String,
      default: "SHOW RESULTS"
    },
    color: {
      type: String,
      default: "#008f84"
    },
    clickedColor: {
      type: String,
      default: "#00504e"
    }
  },
  data: function() {
    return {
      isClick: false,
      mode1: "mode1",
      isMode1: true
    };
  },
  watch: {
    checkMode: {
      immediate: true,
      handler() {
        this.mode == this.mode1
          ? (this.isMode1 = true)
          : (this.isMode1 = false);
      }
    }
  },
  mounted() {
    let root = document.documentElement;
    root.style.setProperty("--colorStyle", this.color);
    root.style.setProperty("--clickedColorStyle", this.clickedColor);
  },
  methods: {
    mouseDown: function(item) {
      this.isClick = !this.isClick;
      this.$emit("click");
    },
    mouseUp: function(item) {
      this.isClick = !this.isClick;
    }
  }
};
</script>

<style>
.SquareButton {
  width: 100%;
  height: 60px;
  border: 1px solid var(--colorStyle);
  -webkit-transition: ease-out 0.4s;
  user-select: none;
  font-family: Muli;
  font-size: 18px;
  font-weight: normal;
  font-style: normal;
  font-stretch: normal;
  line-height: normal;
  letter-spacing: 0.75px;
  text-align: center;
  cursor: pointer;
  display: flex;
  align-items: center;
  justify-content: center;
}

.SquareButton.mode1 {
  color: var(--colorStyle);
  background-color: #ffffff;
  box-shadow: inset 0 0 0 0 var(--colorStyle);
}

.SquareButton {
  color: #ffffff;
  background-color: var(--colorStyle);
  box-shadow: inset 0 0 0 0 #ffffff;
}

.SquareButton.mode1:not(.clicked):hover {
  box-shadow: inset 0 -100px 0 0 var(--colorStyle);
  color: #ffffff;
}

.SquareButton:not(.clicked):hover {
  box-shadow: inset 0 -100px 0 0 #ffffff;
  color: var(--colorStyle);
}

.SquareButton:active {
  background-color: var(--clickedColorStyle);
  color: #ffffff;
  border: 1px solid var(--clickedColorStyle);
}
</style>