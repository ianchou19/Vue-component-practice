<template>
  <div>
    <div class="HeaderContainer">
      <filterBar />
      <banner title="NEWSROOM" type="general" />
    </div>
    <div class="news-content-outer">
      <div class="news-content-inner">
        <div
          v-for="(blok_item, index) in this.story.content.body"
          v-if="index >= cur_page_fst_index && index < (cur_page_fst_index + page_size)"
          class="news_item_container block"
        >
          <img class="news-image" :src="blok_item.news_image" />
          <div class="news-text-container">
            <div class="news-headline-container">
              <a
                :href="blok_item.continue_reading.url"
                class="news-headline"
              >{{blok_item.news_headline}}</a>
            </div>
            <div class="media-and-date-container">
              <div class="media-com-name">{{blok_item.media_com_name}}</div>
              <div class="date">{{blok_item.date}}</div>
            </div>
            <div class="news-text-content">{{blok_item.news_text_content}}</div>
            <a :href="blok_item.continue_reading.url" class="continue-reading">Continue Reading</a>
          </div>
        </div>
      </div>
    </div>
    <PaginationButton
      class="pagination_button"
      :page_num="cur_page_num"
      :last_page="is_last_page"
      @decreasePage="decreasePage"
      @increasePage="increasePage"
    ></PaginationButton>
    <baseSubscribe></baseSubscribe>
  </div>
</template>

<script>
import filterBar from "~/components/FilterBar.vue";
import banner from "~/components/Banner.vue";
import baseSubscribe from "~/components/BaseSubscribe.vue";
import PaginationButton from "~/components/PaginationButton.vue";
export default {
  components: {
    filterBar,
    banner,
    baseSubscribe,
    PaginationButton
  },
  data() {
    return {
      story: { content: {} },
      page_size: 6,
      cur_page_num: 1,
      cur_page_fst_index: 0,
      is_last_page: false,
      isClick: false
    };
  },
  computed: {
    total_page_number() {
      return Math.ceil(this.story.content.body.length / this.page_size);
    }
  },
  methods: {
    increasePage: function() {
      this.cur_page_num += 1;
      this.cur_page_fst_index += this.page_size;
    },
    decreasePage: function() {
      this.cur_page_num -= 1;
      this.cur_page_fst_index -= this.page_size;
    }
  },
  watch: {
    cur_page_num: function(val) {
      if (this.cur_page_num == this.total_page_number) {
        this.is_last_page = true;
      } else {
        this.is_last_page = false;
      }
    }
  },
  mounted() {
    console.log(this.story);
  },
  asyncData(context) {
    // Check if we are in the editor mode
    let version =
      context.query._storyblok || context.isDev ? "draft" : "published";

    // Load the JSON from the API
    return context.app.$storyapi
      .get(`cdn/stories/news`, {
        version: version
      })
      .then(res => {
        return res.data;
      })
      .catch(res => {
        context.error({
          statusCode: res.response.status,
          message: res.response.data
        });
      });
  }
};
</script>

<style>
.news-content-outer {
  display: flex;
  justify-content: center;
  align-items: center;
}

.news-content-inner {
  display: block;
  justify-content: center;
  max-width: 1410px;
  width: 80%;
  height: 100%;
  margin-top: 110px;
  margin-bottom: 70px;
}

.news_item_container {
  display: flex;
}
.news-image {
  width: 23.2%;
}

.news-text-container {
  margin-left: 50px;
}

.news-headline {
  font-family: Taviraj;
  font-size: 28px;
  font-weight: 500;
  font-style: italic;
  font-stretch: normal;
  line-height: 1.29;
  letter-spacing: 0.08px;
  color: #00504e;
}

.news-headline-container {
  margin-bottom: 15px;
}

.media-and-date-container {
  display: flex;
  margin-bottom: 15px;
}

.media-com-name {
  font-family: Muli;
  font-size: 18px;
  font-weight: normal;
  font-style: normal;
  font-stretch: normal;
  line-height: normal;
  letter-spacing: 0.75px;
  color: #00504e;
}

.date {
  font-family: Muli;
  font-size: 16px;
  font-weight: normal;
  font-style: normal;
  font-stretch: normal;
  line-height: normal;
  letter-spacing: 0.67px;
  color: #5f5f5f;
  margin-left: 30px;
}

.news-text-content {
  font-family: Muli;
  font-size: 18px;
  font-weight: 300;
  font-style: normal;
  font-stretch: normal;
  line-height: 1.67;
  letter-spacing: 0.75px;
  color: #5f5f5f;
  margin-bottom: 15px;
}

.continue-reading {
  font-family: Muli;
  font-size: 20px;
  font-weight: 300;
  font-style: italic;
  font-stretch: normal;
  line-height: normal;
  letter-spacing: 0.75px;
  text-align: right;
  color: #008f84;
  height: 25px;
}

.continue-reading:hover {
  font-weight: normal;
  font-style: normal;
}

.continue-reading:active {
  font-weight: bold;
  color: #00504e;
}

.pagination_button {
  margin-bottom: 130px;
}

.block {
  margin-bottom: 60px;
}
</style>
