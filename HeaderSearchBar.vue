<template>
  <div class="topSearchWholeContainer">
    <div class="topSearchContainer" :class="{ typed: isType}">
      <div class="topSearchBarContainer">
        <input
          class="topSearchBarInput"
          :class="{ typed: isType}"
          :placeholder="placeholder"
          v-model="inputText"
          @input="inputChange"
          @keydown="inputKeyDown"
        />
        <div class="topCross">
          <img
            class="topCrossImgNormal"
            :class="{ typed: isType2}"
            src="/delete_icon_top search_field_normal.svg"
            @click="crossClick"
          />
          <img class="topCrossImgClicked" src="/delete_icon_top search_field_clicked.svg" />
        </div>
        <div class="topMagnifier">
          <img
            class="topMagnifierImgNormal"
            :class="{ typed: isClick}"
            src="/search_icon_top_search_field_normal.svg"
            @click="magnifierClick"
          />
          <img
            class="topMagnifierImgClicked"
            :class="{ typed: isClick}"
            src="/search_icon_top_search_field_clicked.svg"
          />
        </div>
      </div>
    </div>
    <div class="topAutocompleteContainer" :class="{ typed: isType, }">
      <li
        v-for="amd_item in autocomplete_data"
        class="topAutoCompListItem"
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
.topSearchWholeContainer {
  margin: 0 auto;
  position: relative;
}

.topSearchContainer {
  width: 100%;
  box-shadow: 0 10px 14px 1px rgba(0, 0, 0, 0.25);
  background-color: #ffffff;
}

.topSearchBarContainer {
  max-width: 74.6%;
  height: 120px;
  display: flex;
  margin-left: auto;
  margin-right: auto;
}

.topSearchBarInput {
  width: 88%;
  height: 85px;
  font-family: Muli;
  font-size: 28px;
  font-weight: normal;
  font-style: normal;
  font-stretch: normal;
  line-height: normal;
  letter-spacing: normal;
  color: #5f5f5f;
  border: none;
  border-bottom: solid 1px #d2af2c;
  outline: none;
}

.topSearchBarInput::placeholder {
  font-family: Muli;
  font-size: 28px;
  font-weight: normal;
  font-style: normal;
  font-stretch: normal;
  line-height: normal;
  letter-spacing: normal;
  opacity: 0.4;
}

.topCross {
  width: 6%;
  height: 85px;
  border: none;
  border-bottom: solid 1px #d2af2c;
  cursor: pointer;
  display: flex;
  align-items: center;
  justify-content: center;
}

.topCrossImgNormal:not(.typed) {
  display: none;
}

.topCrossImgNormal.clicked {
  display: none;
}

.topCrossImgClicked:not(.clicked) {
  display: none;
}

.topMagnifier {
  width: 6%;
  height: 85px;
  border: none;
  border-bottom: solid 1px #d2af2c;
  cursor: pointer;
  display: flex;
  align-items: center;
  justify-content: center;
}

.topMagnifierImgNormal.clicked {
  display: none;
}

.topMagnifierImgClicked:not(.clicked) {
  display: none;
}

.topAutocompleteContainer {
  margin: 0 auto;
  width: 100%;
  height: auto;
  box-shadow: 0 10px 14px 1px rgba(0, 0, 0, 0.25);
  background-color: #ffffff;
  padding-bottom: 4%;
  position: absolute;
  z-index: 1;
}

.topAutocompleteContainer:not(.typed) {
  display: none;
}

.topAutoCompListItem {
  padding-top: 2%;
  padding-bottom: 2%;
  font-family: Muli;
  font-size: 28px;
  font-weight: normal;
  font-style: normal;
  font-stretch: normal;
  line-height: normal;
  letter-spacing: normal;
  color: #5f5f5f;
  cursor: pointer;
  padding-left: 13%;
}

.topAutoCompListItem.selected {
  background: #e6e6e6;
  cursor: pointer;
}
</style>