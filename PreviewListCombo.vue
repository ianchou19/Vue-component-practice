<template>
  <div>
    <div>
      <PreviewList
        :distinction="distinction"
        :col_settings="col_settings"
        :page_size="page_size"
        :data_start_index="data_start_index"
      ></PreviewList>
    </div>
    <div class="bottom_area">
      <LoadMoreButton
        class="LoadMoreButton"
        @clickLoadMore="clickLoadMore"
        @clickClose="clickClose"
      ></LoadMoreButton>
      <PaginationButton
        class="PaginationButton"
        :class="{ pagination_hide: isHide, }"
        :page_num="page_num"
        @decreasePage="decreasePage"
        @increasePage="increasePage"
      />
    </div>
  </div>
</template>

<script>
import PreviewList from "~/components/PreviewList.vue";
import PaginationButton from "~/components/PaginationButton.vue";
import LoadMoreButton from "~/components/LoadMoreButton.vue";

export default {
  components: {
    PreviewList,
    PaginationButton,
    LoadMoreButton
  },
  props: {
    distinction: {
      type: Array
    },
    col_settings: {
      type: Array
    },
    load_more_size: {
      default: 6
    }
  },
  data: function() {
    return {
      default_page_size: 3,
      page_size: 3,
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
      this.page_size = this.load_more_size;
    },
    clickClose: function() {
      this.isHide = !this.isHide;
      this.page_size = this.default_page_size;
    }
  }
};
</script>

<style>
.bottom_area {
  height: 150px;
  padding: 50px 80px 0 80px;
}

.LoadMoreButton {
  float: left;
}

.PaginationButton {
  float: right;
}

.pagination_hide {
  display: none;
}
</style>