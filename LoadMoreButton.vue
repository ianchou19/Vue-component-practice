<template>
  <div class="load_more">
    <label class="load_more_bt" :class="{ clicked: isClick, }" @click="onClick">{{buttonText}}</label>
  </div>
</template>

<script>
export default {
  data: function() {
    return {
      isClick: false,
      buttonText: "LOAD MORE"
    };
  },
  methods: {
    onClick: function() {
      this.isClick = !this.isClick;

      if (this.isClick) {
        this.$emit("clickLoadMore");
        this.buttonText = "CLOSE";
      } else {
        this.$emit("clickClose");
        this.buttonText = "LOAD MORE";
      }
    }
  }
};
</script>

<style>
.load_more_bt {
  height: 20px;
  width: 90px;
  color: #d2af2c;
  background-color: #ffffff;
  border-bottom: solid 1px #d2af2c;
  padding: 0 0 5px 0;
  font-family: Muli;
  font-size: 12px;
  font-weight: bold;
  font-style: normal;
  font-stretch: normal;
  line-height: normal;
  letter-spacing: 0.72px;
  text-align: center;
  cursor: pointer;
}
</style>