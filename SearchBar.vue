<template>
  <div class="searchWholeContainer">
    <div class="searchBarContainer">
      <input
        class="SearchBarInput"
        :class="{ typed: isType}"
        :placeholder="placeholder"
        v-model="inputText"
        @input="inputChange"
        @keydown="inputKeyDown"
      />
      <div class="cross">
        <img
          class="crossImgNormal"
          :class="{typed: isType2}"
          src="/delete_icon_search_field_normal.svg"
          @click="crossClick"
        />
        <img class="crossImgClicked" src="/delete_icon_search_field_clicked.svg" />
      </div>
      <div class="vertical_line"></div>
      <div class="magnifier">
        <img
          class="magnifierImgNormal"
          :class="{ clicked: isClick}"
          src="/search_icon_search_field_normal.svg"
          @click="magnifierClick"
        />
        <img
          class="magnifierImgClicked"
          :class="{ clicked: isClick}"
          src="/search_icon_search_field_hover_and_clicked.svg"
        />
      </div>
    </div>
    <div class="autocompleteContainer" :class="{ typed: isType, }">
      <li
        v-for="amd_item in autocomplete_data"
        class="autoCompListItem"
        :class="{ selected: selectedState[amd_item.id]}"
        :id="amd_item.id"
        style="list-style-type:none;"
        @click="liClick"
        @mouseover="liMouseOver"
      >{{ amd_item.content }}</li>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    autocomplete_data: {
      type: Object
    },
    placeholder: {
      type: String,
      default: "Search for brand, style...etc"
    }
  },
  data: function() {
    return {
      isType: false,
      isType2: false, // isType2 is for the cross icon only
      isClick: false,
      inputText: "",
      selectedState: {}
    };
  },
  methods: {
    inputChange: function(item) {
      if (item.target.value !== "") {
        if (!this.isType) {
          this.isType = true;
          this.isType2 = true;
        }
      } else if (this.isType) {
        this.isType = false;
        this.isType2 = false;
      }

      for (var auto_item in this.autocomplete_data) {
        this.selectedState[this.autocomplete_data[auto_item].id] = false;
      }
      this.$emit("updateSearchQuery", this.inputText);
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
    liMouseOver: function(item) {
      for (var auto_item in this.selectedState) {
        // check if exists any selected autocomplete item
        if (this.selectedState[auto_item]) {
          this.selectedState[auto_item] = false;
          break;
        }
      }
      this.selectedState[item.target.id] = true;
      this.inputText = item.target.innerText;
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
      this.isClick = false;
    },
    crossClick: function(item) {
      this.inputText = "";
      this.isType = false;
      this.isType2 = false;
    },
    magnifierClick: function() {
      this.isClick = true;
      this.onSearch();
    }
  }
};
</script>

<style>
.searchWholeContainer {
  position: relative;
}

.searchBarContainer {
  height: 50px;
  display: flex;
  opacity: 0.8;
  border: solid 2px #d2af2c;
  background-color: #333333;
  text-align: center;
}

.SearchBarInput {
  width: 84%;
  opacity: 0.8;
  border: none;
  background-color: #333333;
  font-family: Muli;
  font-size: 18.4px;
  font-weight: 300;
  font-style: normal;
  font-stretch: normal;
  line-height: normal;
  letter-spacing: normal;
  padding: 12px 0 14px 25px;
  color: #ffffff;
  outline: none;
}

.SearchBarInput::placeholder {
  color: rgba(230, 230, 230, 0.7);
}

.cross {
  width: 8%;
  cursor: pointer;
  display: flex;
  align-items: center;
  justify-content: center;
}

.crossImgNormal:not(.typed) {
  display: none;
}

.crossImgNormal.clicked {
  display: none;
}

.crossImgClicked:not(.clicked) {
  display: none;
}

.vertical_line {
  margin-top: auto;
  margin-bottom: auto;
  border-right: solid 2px #d2af2c;
  height: 75%;
}

.magnifier {
  width: 8%;
  cursor: pointer;
  display: flex;
  align-items: center;
  justify-content: center;
}

.magnifierImgNormal.clicked {
  display: none;
}

.magnifierImgClicked:not(.clicked) {
  display: none;
}

.autocompleteContainer {
  width: 100%;
  background-color: rgba(51, 51, 51, 0.9);
  position: absolute;
  z-index: 1;
}

.autocompleteContainer:not(.typed) {
  display: none;
}

.autoCompListItem {
  color: #ffffff;
  font-family: Muli;
  font-size: 18.4px;
  font-weight: normal;
  font-style: normal;
  font-stretch: normal;
  line-height: normal;
  letter-spacing: normal;
  color: #ffffff;
  cursor: pointer;
  padding: 1.7% 3.7% 1.7% 3.7%;
  position: relative;
}

.autoCompListItem.selected {
  background: #333333;
  cursor: pointer;
}

.autoCompListItem:after {
  content: "";
  position: absolute;
  width: 95%;
  bottom: 0%;
  left: 2.5%;
  border: 1px solid #5f5f5f;
}
</style>