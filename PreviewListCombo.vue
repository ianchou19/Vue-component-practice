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
  data: function() {
    return {
      default_page_size: 3,
      page_size: 3,
      page_num: 1,
      data_start_index: 0,
      load_more_size: 7,
      isHide: false,
      distinction: [
        {
          distinction_title: "Pepsi",
          model_title: "GMT-Master",
          brand_title: "Rolex 1",
          retail_price: null,
          highest_price: "52000.00",
          lowest_price: "8994.00",
          average_price: "20479.35",
          num_sources: "3",
          average_price_change: "1",
          first_date: "2019-04-10",
          recent_date: "2019-06-14",
          first_production_year: "1960",
          last_production_year: "2019",
          image_url:
            "https://stockx.imgix.net/products/watches/Rolex-GMT-Master-II-116719BLRO-Black-front.jpg?fit=fill&bg=FFFFFF&w=700&h=500&auto=format,compress&trim=color&q=90&dpr=2&updated_at=1538080256"
        },
        {
          distinction_title: "Pepsi",
          model_title: "GMT-Master",
          brand_title: "Rolex 2",
          retail_price: null,
          highest_price: "52000.00",
          lowest_price: "8994.00",
          average_price: "20479.35",
          num_sources: "3",
          average_price_change: "1",
          first_date: "2019-04-10",
          recent_date: "2019-06-14",
          first_production_year: "1960",
          last_production_year: "2019",
          image_url:
            "https://stockx.imgix.net/products/watches/Rolex-GMT-Master-II-116719BLRO-Black-front.jpg?fit=fill&bg=FFFFFF&w=700&h=500&auto=format,compress&trim=color&q=90&dpr=2&updated_at=1538080256"
        },
        {
          distinction_title: "Pepsi",
          model_title: "GMT-Master",
          brand_title: "Rolex 3",
          retail_price: null,
          highest_price: "52000.00",
          lowest_price: "8994.00",
          average_price: "20479.35",
          num_sources: "3",
          average_price_change: "1",
          first_date: "2019-04-10",
          recent_date: "2019-06-14",
          first_production_year: "1960",
          last_production_year: "2019",
          image_url:
            "https://stockx.imgix.net/products/watches/Rolex-GMT-Master-II-116719BLRO-Black-front.jpg?fit=fill&bg=FFFFFF&w=700&h=500&auto=format,compress&trim=color&q=90&dpr=2&updated_at=1538080256"
        },
        {
          distinction_title: "Pepsi",
          model_title: "GMT-Master",
          brand_title: "Rolex 4",
          retail_price: null,
          highest_price: "52000.00",
          lowest_price: "8994.00",
          average_price: "20479.35",
          num_sources: "3",
          average_price_change: "1",
          first_date: "2019-04-10",
          recent_date: "2019-06-14",
          first_production_year: "1960",
          last_production_year: "2019",
          image_url:
            "https://stockx.imgix.net/products/watches/Rolex-GMT-Master-II-116719BLRO-Black-front.jpg?fit=fill&bg=FFFFFF&w=700&h=500&auto=format,compress&trim=color&q=90&dpr=2&updated_at=1538080256"
        },
        {
          distinction_title: "Pepsi",
          model_title: "GMT-Master",
          brand_title: "Rolex 5",
          retail_price: null,
          highest_price: "52000.00",
          lowest_price: "8994.00",
          average_price: "20479.35",
          num_sources: "3",
          average_price_change: "1",
          first_date: "2019-04-10",
          recent_date: "2019-06-14",
          first_production_year: "1960",
          last_production_year: "2019",
          image_url:
            "https://stockx.imgix.net/products/watches/Rolex-GMT-Master-II-116719BLRO-Black-front.jpg?fit=fill&bg=FFFFFF&w=700&h=500&auto=format,compress&trim=color&q=90&dpr=2&updated_at=1538080256"
        },
        {
          distinction_title: "Pepsi",
          model_title: "GMT-Master",
          brand_title: "Rolex 6",
          retail_price: null,
          highest_price: "52000.00",
          lowest_price: "8994.00",
          average_price: "20479.35",
          num_sources: "3",
          average_price_change: "1",
          first_date: "2019-04-10",
          recent_date: "2019-06-14",
          first_production_year: "1960",
          last_production_year: "2019",
          image_url:
            "https://stockx.imgix.net/products/watches/Rolex-GMT-Master-II-116719BLRO-Black-front.jpg?fit=fill&bg=FFFFFF&w=700&h=500&auto=format,compress&trim=color&q=90&dpr=2&updated_at=1538080256"
        },
        {
          distinction_title: "Pepsi",
          model_title: "GMT-Master",
          brand_title: "Rolex 7",
          retail_price: null,
          highest_price: "52000.00",
          lowest_price: "8994.00",
          average_price: "20479.35",
          num_sources: "3",
          average_price_change: "1",
          first_date: "2019-04-10",
          recent_date: "2019-06-14",
          first_production_year: "1960",
          last_production_year: "2019",
          image_url:
            "https://stockx.imgix.net/products/watches/Rolex-GMT-Master-II-116719BLRO-Black-front.jpg?fit=fill&bg=FFFFFF&w=700&h=500&auto=format,compress&trim=color&q=90&dpr=2&updated_at=1538080256"
        },
        {
          distinction_title: "Pepsi",
          model_title: "GMT-Master",
          brand_title: "Rolex 8",
          retail_price: null,
          highest_price: "52000.00",
          lowest_price: "8994.00",
          average_price: "20479.35",
          num_sources: "3",
          average_price_change: "1",
          first_date: "2019-04-10",
          recent_date: "2019-06-14",
          first_production_year: "1960",
          last_production_year: "2019",
          image_url:
            "https://stockx.imgix.net/products/watches/Rolex-GMT-Master-II-116719BLRO-Black-front.jpg?fit=fill&bg=FFFFFF&w=700&h=500&auto=format,compress&trim=color&q=90&dpr=2&updated_at=1538080256"
        },
        {
          distinction_title: "Pepsi",
          model_title: "GMT-Master",
          brand_title: "Rolex 9",
          retail_price: null,
          highest_price: "52000.00",
          lowest_price: "8994.00",
          average_price: "20479.35",
          num_sources: "3",
          average_price_change: "1",
          first_date: "2019-04-10",
          recent_date: "2019-06-14",
          first_production_year: "1960",
          last_production_year: "2019",
          image_url:
            "https://stockx.imgix.net/products/watches/Rolex-GMT-Master-II-116719BLRO-Black-front.jpg?fit=fill&bg=FFFFFF&w=700&h=500&auto=format,compress&trim=color&q=90&dpr=2&updated_at=1538080256"
        },
        {
          distinction_title: "Pepsi",
          model_title: "GMT-Master",
          brand_title: "Rolex 10",
          retail_price: null,
          highest_price: "52000.00",
          lowest_price: "8994.00",
          average_price: "20479.35",
          num_sources: "3",
          average_price_change: "1",
          first_date: "2019-04-10",
          recent_date: "2019-06-14",
          first_production_year: "1960",
          last_production_year: "2019",
          image_url:
            "https://stockx.imgix.net/products/watches/Rolex-GMT-Master-II-116719BLRO-Black-front.jpg?fit=fill&bg=FFFFFF&w=700&h=500&auto=format,compress&trim=color&q=90&dpr=2&updated_at=1538080256"
        },
        {
          distinction_title: "Pepsi",
          model_title: "GMT-Master",
          brand_title: "Rolex 11",
          retail_price: null,
          highest_price: "52000.00",
          lowest_price: "8994.00",
          average_price: "20479.35",
          num_sources: "3",
          average_price_change: "1",
          first_date: "2019-04-10",
          recent_date: "2019-06-14",
          first_production_year: "1960",
          last_production_year: "2019",
          image_url:
            "https://stockx.imgix.net/products/watches/Rolex-GMT-Master-II-116719BLRO-Black-front.jpg?fit=fill&bg=FFFFFF&w=700&h=500&auto=format,compress&trim=color&q=90&dpr=2&updated_at=1538080256"
        },
        {
          distinction_title: "Pepsi",
          model_title: "GMT-Master",
          brand_title: "Rolex 12",
          retail_price: null,
          highest_price: "52000.00",
          lowest_price: "8994.00",
          average_price: "20479.35",
          num_sources: "3",
          average_price_change: "1",
          first_date: "2019-04-10",
          recent_date: "2019-06-14",
          first_production_year: "1960",
          last_production_year: "2019",
          image_url:
            "https://stockx.imgix.net/products/watches/Rolex-GMT-Master-II-116719BLRO-Black-front.jpg?fit=fill&bg=FFFFFF&w=700&h=500&auto=format,compress&trim=color&q=90&dpr=2&updated_at=1538080256"
        },
        {
          distinction_title: "Pepsi",
          model_title: "GMT-Master",
          brand_title: "Rolex 13",
          retail_price: null,
          highest_price: "52000.00",
          lowest_price: "8994.00",
          average_price: "20479.35",
          num_sources: "3",
          average_price_change: "1",
          first_date: "2019-04-10",
          recent_date: "2019-06-14",
          first_production_year: "1960",
          last_production_year: "2019",
          image_url:
            "https://stockx.imgix.net/products/watches/Rolex-GMT-Master-II-116719BLRO-Black-front.jpg?fit=fill&bg=FFFFFF&w=700&h=500&auto=format,compress&trim=color&q=90&dpr=2&updated_at=1538080256"
        },
        {
          distinction_title: "Pepsi",
          model_title: "GMT-Master",
          brand_title: "Rolex 14",
          retail_price: null,
          highest_price: "52000.00",
          lowest_price: "8994.00",
          average_price: "20479.35",
          num_sources: "3",
          average_price_change: "1",
          first_date: "2019-04-10",
          recent_date: "2019-06-14",
          first_production_year: "1960",
          last_production_year: "2019",
          image_url:
            "https://stockx.imgix.net/products/watches/Rolex-GMT-Master-II-116719BLRO-Black-front.jpg?fit=fill&bg=FFFFFF&w=700&h=500&auto=format,compress&trim=color&q=90&dpr=2&updated_at=1538080256"
        },
        {
          distinction_title: "Pepsi",
          model_title: "GMT-Master",
          brand_title: "Rolex 15",
          retail_price: null,
          highest_price: "52000.00",
          lowest_price: "8994.00",
          average_price: "20479.35",
          num_sources: "3",
          average_price_change: "1",
          first_date: "2019-04-10",
          recent_date: "2019-06-14",
          first_production_year: "1960",
          last_production_year: "2019",
          image_url:
            "https://stockx.imgix.net/products/watches/Rolex-GMT-Master-II-116719BLRO-Black-front.jpg?fit=fill&bg=FFFFFF&w=700&h=500&auto=format,compress&trim=color&q=90&dpr=2&updated_at=1538080256"
        },
        {
          distinction_title: "Pepsi",
          model_title: "GMT-Master",
          brand_title: "Rolex 16",
          retail_price: null,
          highest_price: "52000.00",
          lowest_price: "8994.00",
          average_price: "20479.35",
          num_sources: "3",
          average_price_change: "1",
          first_date: "2019-04-10",
          recent_date: "2019-06-14",
          first_production_year: "1960",
          last_production_year: "2019",
          image_url:
            "https://stockx.imgix.net/products/watches/Rolex-GMT-Master-II-116719BLRO-Black-front.jpg?fit=fill&bg=FFFFFF&w=700&h=500&auto=format,compress&trim=color&q=90&dpr=2&updated_at=1538080256"
        },
        {
          distinction_title: "Pepsi",
          model_title: "GMT-Master",
          brand_title: "Rolex 17",
          retail_price: null,
          highest_price: "52000.00",
          lowest_price: "8994.00",
          average_price: "20479.35",
          num_sources: "3",
          average_price_change: "1",
          first_date: "2019-04-10",
          recent_date: "2019-06-14",
          first_production_year: "1960",
          last_production_year: "2019",
          image_url:
            "https://stockx.imgix.net/products/watches/Rolex-GMT-Master-II-116719BLRO-Black-front.jpg?fit=fill&bg=FFFFFF&w=700&h=500&auto=format,compress&trim=color&q=90&dpr=2&updated_at=1538080256"
        }
      ],
      col_settings: [
        { title: "", type: "heart" },
        { title: "", type: "image" },
        { title: "Brand/Model", type: "text" },
        { title: "Average Price", type: "text" },
        { title: "Lowest Price", type: "text" },
        { title: "Highest Price", type: "text" }
      ]
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