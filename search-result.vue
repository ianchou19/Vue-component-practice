<template>
  <section class="container">
    <div class="BannerContainer">
      <Banner :title="title" type="general" />
    </div>
    <FilterBar />
    <div class="contentContainer">
      <div class="PriceGraphArea">
        <PriceGraph
          :id="1"
          fetchLink="https://y5n0m8lndb.execute-api.us-east-1.amazonaws.com/alpha/pricedata?model_title=GMT-Master&distinction_title=Batman"
        />
      </div>
      <div class="GallerySwitchContainer">
        <BaseGalleryListSwitch @view="changeView" />
      </div>
      <div class="CategorySection">
        <div class="SectionHeaderContainer">
          <div class="SectionTitle">MODELS</div>
          <div>
            <SortBy :items="items" :defaultSort="sortModel" @sortBy="changeSortModel" />
          </div>
        </div>
        <div v-if="view=='gallery'" class="WatchContent">
          <div class="ArrowButtonContainer">
            <div class="LeftArrow">
              <ArrowButtonLeft @clickLeftArrow="decreaseModelPage" />
            </div>
            <div class="RightArrow">
              <ArrowButtonRight @clickRightArrow="increaseModelPage" />
            </div>
          </div>
          <MultiplePreviewSectionThree :products="models" rownum="row1" ref="threeWatches1" />
        </div>
        <div v-else class="WatchContent">
          <div class="PreviewListContainer">
            <PreviewListCombo :distinction="models" page="watch" />
          </div>
        </div>
      </div>
      <div class="Divider"></div>
      <div class="CategorySection">
        <div class="SectionHeaderContainer">
          <div class="SectionTitle">DISTINCTIONS</div>
          <div class="SortBy-2">
            <SortBy :items="items" :defaultSort="sortDist" @sortBy="changeSortDist" />
          </div>
        </div>
        <div v-if="view=='gallery'" class="WatchContent">
          <div class="ArrowButtonContainer">
            <div class="LeftArrow">
              <ArrowButtonLeft @clickLeftArrow="decreaseDistPage" />
            </div>
            <div class="RightArrow">
              <ArrowButtonRight @clickRightArrow="increaseDistPage" />
            </div>
          </div>
          <MultiplePreviewSectionThree :products="distinctions" rownum="row2" ref="threeWatches1" />
        </div>
        <div v-else class="WatchContent">
          <div class="PreviewListContainer">
            <PreviewListCombo :distinction="distinctions" page="watch" />
          </div>
        </div>
      </div>
      <div class="Divider"></div>
      <div class="CategorySection">
        <div class="SectionHeaderContainer">
          <div class="SectionTitle">WATCHES</div>
          <div class="SortBy-2">
            <SortBy :items="itemsWatch" :defaultSort="sortWatch" @sortBy="changeSortWatch" />
          </div>
        </div>
        <div v-if="view=='gallery'" class="WatchContent">
          <div class="ArrowButtonContainer">
            <div class="LeftArrow">
              <ArrowButtonLeft @clickLeftArrow="decreaseWatchPage" />
            </div>
            <div class="RightArrow">
              <ArrowButtonRight @clickRightArrow="increaseWatchPage" />
            </div>
          </div>
          <MultiplePreviewSectionThree :products="watches" rownum="row3" ref="threeWatches1" />
        </div>
        <div v-else class="WatchContent">
          <div class="PreviewListContainer">
            <PreviewListCombo :distinction="watches" page="watch" />
          </div>
        </div>
      </div>
      <div class="SubscribeContainer">
        <BaseSubscribe></BaseSubscribe>
      </div>
    </div>
  </section>
</template>

<script>
import Banner from "~/components/Banner.vue";
import FilterBar from "~/components/FilterBar.vue";
import PriceGraph from "~/components/PriceGraph.vue";
import BaseGalleryListSwitch from "~/components/BaseGalleryListSwitch.vue";
import SortBy from "~/components/SortBy.vue";
import ArrowButtonLeft from "~/components/ArrowButtonLeft.vue";
import ArrowButtonRight from "~/components/ArrowButtonRight.vue";
import MultiplePreviewSectionThree from "~/components/MultiplePreviewSectionThree.vue";
import PreviewListCombo from "~/components/PreviewListCombo.vue";
import BaseSubscribe from "~/components/BaseSubscribe.vue";

export default {
  components: {
    Banner,
    FilterBar,
    PriceGraph,
    BaseGalleryListSwitch,
    SortBy,
    ArrowButtonLeft,
    ArrowButtonRight,
    MultiplePreviewSectionThree,
    PreviewListCombo,
    BaseSubscribe
  },
  data: function() {
    return {
      searchResult: "ROLEX",
      title: "SEARCH RESULT FOR: ",
      view: "gallery",
      models: [],
      distinctions: [],
      watches: [],
      modelsPageNum: 1,
      distsPageNum: 1,
      watchesPageNum: 1,
      sortModel: "% return",
      sortDist: "% return",
      sortWatch: "% return",
      items: ["% return", "relevance"],
      itemsWatch: [
        "date added to site",
        "year released",
        "% return",
        "relevance"
      ]
    };
  },
  methods: {
    changeView(value) {
      console.log(value);
      this.view = value;
    },
    decreaseModelPage: function() {
      this.modelsPageNum -= 1;
    },
    increaseModelPage: function() {
      this.modelsPageNum += 1;
    },
    decreaseDistPage: function() {
      this.distsPageNum -= 1;
    },
    increaseDistPage: function() {
      this.distsPageNum += 1;
    },
    decreaseWatchPage: function() {
      this.watchesPageNum -= 1;
    },
    increaseWatchPage: function() {
      this.watchesPageNum += 1;
    },
    changeSortModel(value) {
      console.log(value);
      this.sortModel = value;
    },
    changeSortDist(value) {
      console.log(value);
      this.sortDist = value;
    },
    changeSortWatch(value) {
      console.log(value);
      this.sortWatch = value;
    }
  },
  watch: {
    modelsPageNum: function() {
      fetch(
        `https://y5n0m8lndb.execute-api.us-east-1.amazonaws.com/alpha/watches?brand_title=Rolex&num=3&page=${this.modelsPageNum}`,
        {
          headers: {
            "x-api-key": "l5JnZBhvMG22sknSiM0cv7NoZ3cEaq2G4zrhHapt"
          }
        }
      )
        .then(function(response) {
          return response.json();
        })
        .then(arrOfProducts => {
          this.models = arrOfProducts;
        })
        .catch(err => {
          console.log(err);
        });
    },
    distsPageNum: function() {
      fetch(
        `https://y5n0m8lndb.execute-api.us-east-1.amazonaws.com/alpha/watches?brand_title=Rolex&num=3&page=${this.distsPageNum}`,
        {
          headers: {
            "x-api-key": "l5JnZBhvMG22sknSiM0cv7NoZ3cEaq2G4zrhHapt"
          }
        }
      )
        .then(function(response) {
          return response.json();
        })
        .then(arrOfProducts => {
          this.distinctions = arrOfProducts;
        })
        .catch(err => {
          console.log(err);
        });
    },
    watchesPageNum: function() {
      fetch(
        `https://y5n0m8lndb.execute-api.us-east-1.amazonaws.com/alpha/watches?brand_title=Rolex&num=3&page=${this.watchesPageNum}`,
        {
          headers: {
            "x-api-key": "l5JnZBhvMG22sknSiM0cv7NoZ3cEaq2G4zrhHapt"
          }
        }
      )
        .then(function(response) {
          return response.json();
        })
        .then(arrOfProducts => {
          this.watches = arrOfProducts;
        })
        .catch(err => {
          console.log(err);
        });
    }
  },
  beforeMount: function() {
    this.title = this.title + this.searchResult;
    fetch(
      `https://y5n0m8lndb.execute-api.us-east-1.amazonaws.com/alpha/watches?brand_title=Rolex&num=3&page=${this.modelsPageNum}`,
      {
        headers: {
          "x-api-key": "l5JnZBhvMG22sknSiM0cv7NoZ3cEaq2G4zrhHapt"
        }
      }
    )
      .then(function(response) {
        return response.json();
      })
      .then(arrOfProducts => {
        this.models = arrOfProducts;
      })
      .catch(err => {
        console.log(err);
      });
    fetch(
      `https://y5n0m8lndb.execute-api.us-east-1.amazonaws.com/alpha/watches?brand_title=Rolex&num=3&page=${this.distsPageNum}`,
      {
        headers: {
          "x-api-key": "l5JnZBhvMG22sknSiM0cv7NoZ3cEaq2G4zrhHapt"
        }
      }
    )
      .then(function(response) {
        return response.json();
      })
      .then(arrOfProducts => {
        this.distinctions = arrOfProducts;
      })
      .catch(err => {
        console.log(err);
      });
    fetch(
      `https://y5n0m8lndb.execute-api.us-east-1.amazonaws.com/alpha/watches?brand_title=Rolex&num=3&page=${this.watchesPageNum}`,
      {
        headers: {
          "x-api-key": "l5JnZBhvMG22sknSiM0cv7NoZ3cEaq2G4zrhHapt"
        }
      }
    )
      .then(function(response) {
        return response.json();
      })
      .then(arrOfProducts => {
        this.watches = arrOfProducts;
      })
      .catch(err => {
        console.log(err);
      });
  }
};
</script>

<style lang="scss">
@import url("https://fonts.googleapis.com/css?family=Muli:200,400,700|Taviraj:400,400i,500i&display=swap");

.container {
  margin: 0;
  min-height: 100vh;
  display: flex;
  flex-direction: column;
  align-items: center;
}

.BannerContainer {
  width: 100vw;
}

.contentContainer {
  max-width: 1763px;
  width: 100%;
  display: flex;
  flex-direction: column;
  align-items: center;
}

.PriceGraphArea {
  margin: 0 auto;
  max-width: 1410px;
  width: 80%;
  margin-top: 80px;
}

.GallerySwitchContainer {
  margin-top: 67px;
  margin-right: -10px;
  max-width: 1410px;
  width: 80%;
  display: flex;
  justify-content: flex-end;
}

.CategorySection {
  max-width: 1763px;
  width: 100%;
  display: flex;
  flex-direction: column;
  align-items: center;
  margin-bottom: 120px;
  margin-top: 95px;
}

.SectionHeaderContainer {
  display: flex;
  width: 80%;
  justify-content: space-between;
}

.SectionTitle {
  font-family: Muli;
  font-size: 48px;
  font-weight: 300;
  font-style: normal;
  font-stretch: normal;
  line-height: normal;
  letter-spacing: 2px;
  text-align: center;
  color: $--pea;
  margin-bottom: 60px;
}

.SortBy-2 {
  margin-top: -55px;
}

.WatchContent {
  width: 100%;
}

.PreviewListContainer {
  margin: 0 auto;
  max-width: 1410px;
  margin-top: 35px;
}

.Divider {
  width: 80%;
  height: 2px;
  border: solid 1px $--very-light-pink;
}

.title {
  font-family: "Quicksand", "Source Sans Pro", -apple-system, BlinkMacSystemFont,
    "Segoe UI", Roboto, "Helvetica Neue", Arial, sans-serif;
  display: block;
  font-weight: 300;
  font-size: 100px;
  color: $--very-light-pink;
  letter-spacing: 1px;
}

.subtitle {
  font-weight: 300;
  font-size: 42px;
  color: #526488;
  word-spacing: 5px;
  padding-bottom: 15px;
  margin-bottom: 50px;
}

.links {
  padding-top: 15px;
}

.ArrowButtonContainer {
  position: relative;
  max-width: 1763px;
}
.LeftArrow {
  position: absolute;
  left: 35px;
  top: 235px;
  width: 84px;
  height: 84px;
}
.RightArrow {
  position: absolute;
  right: 36px;
  top: 235px;
  width: 84px;
  height: 84px;
}

.SubscribeContainer {
  width: 100%;
}
</style>
