<template>
  <div class="banner" :class="type">
    <div v-if="type!='home'" class="breadcrumbContainer">
      <Breadcrumb :mode="'mode2'" :label_list="label_list" />
    </div>
    <div class="banner-title">
      <span class="banner-title-text">{{title}}</span>
    </div>
    <div v-if="type=='home'" class="searchBar">
      <div class="bannerSearch">
        <searchbar placeholder="Search for brand, style… etc" :autocomplete_data="temp_data" />
      </div>
    </div>
    <div v-if="type=='faq'" class="searchBar-faq">
      <div class="bannerSearch-faq">
        <searchbar
          @updateSearchQuery="updateFAQResults"
          placeholder="Search for answers"
          :autocomplete_data="searchResults"
        />
      </div>
    </div>
  </div>
</template>

<script>
import searchbar from "~/components/SearchBar.vue";
import Breadcrumb from "~/components/Breadcrumb.vue";

export default {
  props: ["title", "type"],
  components: {
    searchbar,
    Breadcrumb
  },
  data: function() {
    return {
      stories: {},
      searchText: "",
      label_list: [
        {
          content: "HOME",
          url: "http://styleguide.wesearchwatches.com/#breadcrumb"
        },
        {
          content: "ALL MODELS",
          url: "http://styleguide.wesearchwatches.com/#breadcrumb"
        },
        {
          content: "MODEL NAME",
          url: "http://styleguide.wesearchwatches.com/#breadcrumb"
        }
      ],
      temp_data: {
        fakeData_1: {
          id: "item_1",
          content: "apple"
        },
        fakeData_2: {
          id: "item_2",
          content: "apple farmer"
        },
        fakeData_3: {
          id: "item_3",
          content: "apple is delicious"
        },
        fakeData_4: {
          id: "item_4",
          content: "apple apple apple!"
        },
        fakeData_5: {
          id: "item_5",
          content: "apple can save your life!!!!!"
        }
      }
    };
  },
  methods: {
    updateFAQResults: function(searchText) {
      this.searchText = searchText;
      let fetchLink = `https://api.storyblok.com/v1/cdn/stories?token=3geO4fuHwC97RFah60MwHQtt&search_term=${searchText}`;
      return fetch(fetchLink, {
        headers: {
          Accept: "application/json",
          "Content-Type": "application/json"
        }
      })
        .then(function(response) {
          return response.json();
        })
        .then(stories => {
          console.log(fetchLink);
          console.log(stories);
          this.stories = stories;
        });
    }
  },
  computed: {
    searchResults: function() {
      const vm = this;
      let searchContent = [];
      let resultsObject = {};
      if (this.stories.stories !== undefined) {
        searchContent = this.stories.stories.reduce(function(arr, story) {
          if (story.name !== "faq") {
            return [].concat(story.content.body[0].blocks);
          }
        }, []);
        if (searchContent != undefined) {
          searchContent = searchContent.filter(function(item) {
            return (
              item.answer.toLowerCase().indexOf(vm.searchText.toLowerCase()) !==
                -1 ||
              item.title.toLowerCase().indexOf(vm.searchText.toLowerCase()) !==
                -1
            );
          });
          console.log(searchContent);
          for (let i = 0; i < searchContent.length; i++) {
            resultsObject[i] = {
              id: i,
              content: searchContent[i].title,
              answer: searchContent[i].answer
            };
          }
        }
      }
      const parsed = JSON.stringify(resultsObject);
      try {
        localStorage.setItem("FAQSearchResults", parsed);
      } catch {
        console.log("hello");
      }
      return resultsObject;
    }
  }
};
</script>
<style>
.bannerSearch {
  width: 723px;
  height: 54px;
}
.searchBar {
  display: flex;
  justify-content: left;
  max-width: 1410px;
  width: 100%;
}
.bannerSearch-faq {
  width: 929px;
  height: 54px;
  margin-top: 50px;
}
.searchBar-faq {
  display: flex;
  justify-content: center;
  max-width: 1410px;
  width: 100%;

  text-align: center;
  align-items: center;
}
.banner.general {
  height: 200px;
  background-blend-mode: multiply;
  background-image: linear-gradient(to bottom, #5f5f5f, #5f5f5f);
  text-align: center;
  display: flex;
  justify-content: center;
  align-items: center;
  background: url("/banner bg.png");
  background-size: cover;
}
.banner.home {
  height: 662px;
  background-blend-mode: multiply;
  background-image: linear-gradient(to bottom, #5f5f5f, #5f5f5f);
  justify-content: center;
  display: flex;
  flex-direction: column;
  align-items: center;
  background: url("/homepage banner bg.png");
  background-size: cover;
}
.banner.faq {
  height: 320px;
  background-blend-mode: multiply;
  background-image: linear-gradient(to bottom, #5f5f5f, #5f5f5f);
  text-align: center;
  display: flex;
  justify-content: center;
  flex-direction: column;
  align-items: center;
  background: url("/banner bg.png");
  background-size: cover;
}

.banner-title {
  display: flex;
  justify-content: center;
  max-width: 1410px;
  width: 100%;

  text-align: center;
  align-items: center;
}
.banner.home > .banner-title {
  display: flex;
  justify-content: left;
  max-width: 1410px;
  width: 100%;
  text-align: left;

  align-items: center;
}
.banner.home > .banner-title > .banner-title-text {
  width: 814px;
  height: 260px;
  font-family: Taviraj;
  font-size: 62px;
  font-weight: 300;
  font-style: italic;
  font-stretch: normal;
  line-height: 1.24;
  letter-spacing: 0.53px;
  color: #ffffff;
}

.banner-title-text {
  font-family: Muli;
  font-size: 48px;
  font-weight: 300;
  font-style: normal;
  font-stretch: normal;
  line-height: normal;
  letter-spacing: 2px;
  user-select: none;
  color: #d2af2c;
}

.breadcrumbContainer {
  width: 74.6%;
  height: inherit;
  position: absolute;
}
</style>