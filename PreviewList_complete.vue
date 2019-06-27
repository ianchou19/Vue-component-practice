<template>
  <div>
    <div class="top_title">
      <div id="col_1" class="col_title">{{title.col_1}}</div>
      <div id="col_2" class="col_title">{{title.col_2}}</div>
      <div id="col_3" class="col_title">{{title.col_3}}</div>
      <div id="col_4" class="col_title">{{title.col_4}}</div>
    </div>
    <div
      v-for="(dist,index) in distinction"
      v-if="index >= data_start_index && index < (data_start_index + page_actual_size)"
    >
      <PreviewListRow :product="dist"/>
    </div>
    <div class="pagination_pos">
      <LoadMoreButton @clickLoadMore="clickLoadMore" @clickClose="clickClose"></LoadMoreButton>
      <PaginationButton
        :class="{ pagination_hide: isHide, }"
        :page_num="page_num"
        @decreasePage="decreasePage"
        @increasePage="increasePage"
      />
    </div>
  </div>
</template>

<script>
import PreviewListRow from "~/components/PreviewListRow.vue";
import PaginationButton from "~/components/PaginationButton.vue";
import LoadMoreButton from "~/components/LoadMoreButton.vue";

export default {
  components: {
    PreviewListRow,
    PaginationButton,
    LoadMoreButton
  },
  props: {
    distinction: Object,
    page_size: {
      default: 3
    }
  },
  data: function() {
    return {
      title: {
        col_1: "Brand/Model",
        col_2: "Average Price",
        col_3: "Lowest Price",
        col_4: "Highest Price"
      },
      page_actual_size: this.page_size,
      page_num: 1,
      data_start_index: 0,
      isHide: false
    };
  },
  methods: {
    increasePage: function() {
      var total_page_number = Math.ceil(
        this.distinction.length / this.page_size
      );
      if (this.page_num == total_page_number) return;

      this.page_num += 1;
      this.data_start_index += this.page_size;
    },
    decreasePage: function() {
      if (this.page_num == 1) return;

      this.page_num -= 1;
      this.data_start_index -= this.page_size;
    },
    clickLoadMore: function() {
      this.isHide = !this.isHide;
      this.page_actual_size = 5;
    },
    clickClose: function() {
      this.isHide = !this.isHide;
      this.page_actual_size = this.page_size;
    }
  }
};
</script>

<style>
.top_title {
  display: flex;
  border-bottom: 1px solid #333333;
  padding: 0 0 33px 0;
  width: 1320px;
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

#col_1 {
  width: 660px;
  padding: 0 40px 0 300px;
}

#col_2 {
  width: 220px;
  padding: 0 30px 0 0;
}

#col_3 {
  width: 220px;
  padding: 0 30px 0 0;
}

#col_4 {
  width: 220px;
  padding: 0 30px 0 0;
}

.pagination_pos {
  height: 150px;
  padding: 50px 0 0 200px;
  display: flex;
}

.pagination_hide {
  display: none;
}
</style>