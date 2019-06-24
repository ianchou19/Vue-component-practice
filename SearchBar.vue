<template>
  <div class="searchContainer">
    <div class="searchBarContainer">
      <input
        :class="{ typed: isType, }"
        class="SearchBarInput"
        :placeholder="placeholder"
        v-model="inputText"
        @input="inputChange"
        @keydown="inputKeyDown"
      >
      <div class="cross">
        <img
          :class="{ typed: isClick, }"
          class="crossImgNormal"
          src="/delete_icon_search_field_normal.svg"
          @click="crossClick"
        >
        <img
          :class="{ typed: isClick, }"
          class="crossImgClicked"
          src="/delete_icon_search_field_clicked.svg"
        >
      </div>
      <div class="magnifier">
        <img
          :class="{ typed: isClick, }"
          class="magnifierImgNormal"
          src="/search_icon_search_field_normal.svg"
          @click="magnifierClick"
        >
        <img
          :class="{ typed: isClick, }"
          class="magnifierImgClicked"
          src="/search_icon_search_field_hover_and_clicked.svg"
        >
      </div>
    </div>
    <div :class="{ typed: isType, }" class="autocompleteContainer">
      <li
        v-for="amd_item in autocomplete_data"
        class="autoCompListItem"
        :class="{ selected: selectedState[amd_item.id]}"
        :id="amd_item.id"
        style="list-style-type:none;"
        @click="liClick"
      >{{ amd_item.content }}</li>
    </div>
  </div>
</template>

<script>
export default {
  data: function() {
    return {
      isType: false,
      isClick: false,
      inputText: "",
      placeholder: "Search for brand, style...etc",
      selectedState: {},
      autocomplete_data: {
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
          content: "apple can save your life"
        }
      }
    };
  },
  methods: {
    inputChange: function(item) {
      if (item.target.value !== "") {
        if (!this.isType) {
          this.isType = true;
        }
      } else if (this.isType) {
        this.isType = false;
      }

      for (var auto_item in this.autocomplete_data) {
        this.selectedState[this.autocomplete_data[auto_item].id] = false;
      }
    },
    inputKeyDown: function(item) {
      if (item.key === "ArrowDown") {
        this.moveSelection("down");
      } else if (item.key === "ArrowUp") {
        this.moveSelection("up");
      } else if (item.key === "Enter") {
        this.onSearch();
      }
    },
    liClick: function(item) {
      for (var auto_item in this.selectedState) {
        this.selectedState[auto_item] = false;
      }
      this.selectedState[item.target.id] = true;
      this.inputText = item.target.innerText;
      this.onSearch();
    },
    moveSelection: function(direction) {
      // block the Key operation when autocomplete sction is closed
      if (!this.isType) return;

      if (direction === "up") {
        direction = -1;
      } else if (direction === "down") {
        direction = 1;
      }

      var flag = false;
      var selected_item;
      for (var auto_item in this.selectedState) {
        // check if exists any selected autocomplete item
        if (this.selectedState[auto_item]) {
          flag = true;
          selected_item = auto_item;
          break;
        }
      }

      if (!flag) {
        selected_item = "item_1";
        this.selectedState[selected_item] = true; // when no autocomplete item has been selected
      } else {
        var new_position = parseInt(selected_item.slice(-1)) + direction;
        var total_auto_item = Object.keys(this.selectedState).length;

        // restrict the movement only between uppest and lowest autocomplete item
        if (new_position > 0 && new_position <= total_auto_item) {
          this.selectedState[selected_item] = false;
          selected_item = selected_item.slice(0, -1) + new_position.toString();
          this.selectedState[selected_item] = true;
        }
      }

      for (var data_item in this.autocomplete_data) {
        if (this.autocomplete_data[data_item].id === selected_item) {
          // add the content of selected item into the Input element
          this.inputText = this.autocomplete_data[data_item].content;
          break;
        }
      }
    },
    onSearch: function() {
      console.log("on search!");
      this.isType = false;
    },
    crossClick: function(item) {
      this.inputText = "";
      this.isType = false;
    },
    magnifierClick: function() {
      this.onSearch();
    }
  }
};
</script>

<style>
.searchBarContainer {
  width: 723px;
  height: 50px;
  opacity: 0.8;
  border: solid 1px #d2af2c;
  background-color: #333333;
  display: flex;
}

.SearchBarInput {
  width: 588px;
  opacity: 0.8;
  border: solid 1px #d2af2c;
  border-bottom: solid 2px #d2af2c;
  border-right-color: #333333;
  background-color: #333333;
  font-family: Muli;
  font-size: 18.4px;
  font-weight: 300;
  font-style: normal;
  font-stretch: normal;
  line-height: normal;
  letter-spacing: normal;
  color: rgba(230, 230, 230, 0.7);
  padding: 13px 0 14px 25px;
}

.SearchBarInput.typed {
  color: #ffffff;
}

.cross {
  /* height: 47px; */
  border-top: solid 1px #d2af2c;
  border-right: solid 1px #d2af2c;
  border-bottom: solid 2px #d2af2c;
  cursor: pointer;
}

.crossImgNormal {
  padding: 13px 20px 0 20px;
}

.crossImgNormal.typed {
  display: none;
}

.crossImgClicked {
  padding: 13px 20px 0 20px;
}

.crossImgClicked:not(.typed) {
  display: none;
}

.magnifier {
  width: 72px;
  /* height: 47px; */
  border-top: solid 1px #d2af2c;
  border-right: solid 1px #d2af2c;
  border-bottom: solid 2px #d2af2c;
  cursor: pointer;
}

.magnifierImgNormal {
  padding: 13px 0 0 25px;
}

.magnifierImgNormal.typed {
  display: none;
}

.magnifierImgClicked {
  padding: 13px 20px 0 20px;
}

.magnifierImgClicked:not(.typed) {
  display: none;
}

.autocompleteContainer {
  width: 723px;
  background-color: rgba(51, 51, 51, 0.9);
}

.autocompleteContainer:not(.typed) {
  display: none;
}

.autoCompListItem {
  color: #ffffff;
  padding-top: 15px;
  padding-left: 25px;
  padding-bottom: 15px;
  border-bottom: 1px solid #5f5f5f;
  font-family: Muli;
  font-size: 18.4px;
  font-weight: normal;
  font-style: normal;
  font-stretch: normal;
  line-height: normal;
  letter-spacing: normal;
  color: #ffffff;
  cursor: pointer;
}

.autoCompListItem.selected {
  background: #333333;
  cursor: pointer;
}
</style>