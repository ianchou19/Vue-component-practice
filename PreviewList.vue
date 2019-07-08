<template>
  <div class="preview_list">
    <div class="title_row">
      <div class="col_title" :class="col.type" v-for="col in col_settings">{{col.title}}</div>
    </div>
    <div
      v-for="(dist,index) in distinction"
      v-if="index >= data_start_index && index < (data_start_index + page_size)"
    >
      <PreviewListRow class="PreviewListRow" :product="dist"/>
    </div>
  </div>
</template>

<script>
import PreviewListRow from "~/components/PreviewListRow.vue";

export default {
  components: {
    PreviewListRow
  },
  props: {
    distinction: {
      type: Array
    },
    col_settings: {
      type: Array
    },
    page_size: {
      default: 1000
    },
    data_start_index: {
      default: 0
    }
  }
};
</script>

<style>
.title_row {
  display: flex;
  border-bottom: 1px solid #333333;
  padding: 0 0 33px 0;
}

.col_title {
  font-family: Muli;
  font-size: 20px;
  font-weight: bold;
  font-style: normal;
  font-stretch: normal;
  line-height: normal;
  letter-spacing: 0.2px;
  color: #333333;
  word-wrap: break-word;
}

/* .col_title.heart {
  width: 30%;
}

.col_title.image {
  width: 30%;
}

.col_title.text {
  width: 30%;
}

.col_title.button {
  width: 30%;
} */

.PreviewListRow {
  border-bottom: solid 1px #e6e6e6;
}
</style>