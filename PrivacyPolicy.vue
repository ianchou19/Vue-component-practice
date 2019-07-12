<template>
  <div class="pp-container">
    <banner title="Privacy Policy" type="general" />
    <div class="pp-content">
      <div class="pp-content-block">
        <div class="pp-content-title">{{blok.title_1}}</div>
        <div class="pp-content-regular">
          <p>{{blok.content_1}}</p>
        </div>
      </div>
      <div class="pp-content-block">
        <div class="pp-content-title">{{blok.title_2}}</div>
        <div class="pp-content-regular">
          <p>{{blok.content_2}}</p>
        </div>
      </div>
      <div class="pp-content-block">
        <div class="pp-content-title">{{blok.title_3}}</div>
        <div class="pp-content-regular">
          <p>{{blok.content_3}}</p>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import banner from "~/components/Banner.vue";
export default {
  props: ["blok"],
  components: {
    banner
  }
};
</script>

<style>
.pp-container {
  -moz-osx-font-smoothing: grayscale;
  -webkit-font-smoothing: antialiased;
}

.pp-content {
  height: 500px;
  display: flex;
  justify-content: center;
  align-items: center;
}

.pp-content-block {
  display: block;
  justify-content: center;
  max-width: 1410px;
  width: 80%;
  height: 80%;
}

.pp-content-title {
  font-family: Muli;
  font-size: 20px;
  font-weight: normal;
  font-style: normal;
  font-stretch: normal;
  line-height: normal;
  letter-spacing: 0.8px;
  color: #00504e;
  max-width: 1410px;
  width: 100%;
  text-transform: uppercase;
  margin-bottom: 10px;
}

.pp-content-regular {
  font-family: Muli;
  font-size: 18px;
  font-weight: 300;
  font-style: normal;
  font-stretch: normal;
  line-height: 1.67;
  letter-spacing: 0.75px;
  color: #5f5f5f;
  max-width: 1410px;
  width: 100%;
}
</style>
