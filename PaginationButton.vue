<template>
  <div class="pagination">
    <div class="left_arrow">
      <ArrowButtonLeft @clickLeftArrow="decreasePage" />
    </div>
    <label class="page_info page_pre">{{ page_prefix }}</label>
    <label class="page_info page_num">{{ page_num }}</label>
    <div class="right_arrow">
      <ArrowButtonRight @clickRightArrow="increasePage" />
    </div>
  </div>
</template>

<script>
import ArrowButtonLeft from "~/components/ArrowButtonLeft.vue";
import ArrowButtonRight from "~/components/ArrowButtonRight.vue";

export default {
  components: {
    ArrowButtonLeft,
    ArrowButtonRight
  },
  props: {
    page_num: {
      default: 1
    }
  },
  data: function() {
    return {
      page_prefix: "PAGE"
    };
  },
  methods: {
    increasePage: function() {
      this.$emit("increasePage");
    },
    decreasePage: function() {
      this.$emit("decreasePage");
    }
  }
};
</script>

<style>
.pagination {
  display: flex;
  justify-content: center;
  align-items: center;
}

.left_arrow {
  width: 40px;
  height: 40px;
}

.right_arrow {
  width: 40px;
  height: 40px;
}

.page_info {
  color: #d2af2c;
  font-family: Muli;
  font-size: 12px;
  font-weight: bold;
  font-style: normal;
  font-stretch: normal;
  line-height: normal;
  letter-spacing: 0.72px;
  text-align: center;
}

.page_pre {
  padding: 0 10px 0 44px;
}

.page_num {
  padding: 0 44px 0 0;
}

.left_arrow {
  width: 40px;
  height: 40px;
}

.right_arrow {
  width: 40px;
  height: 40px;
}
</style>