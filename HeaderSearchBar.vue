<template>
  <div class="topSearchContainer" :class="{ typed: isType, }">
    <div class="topSearchBarContainer">
      <input
        class="topSearchBarInput"
        :class="{ typed: isType, }"
        :placeholder="placeholder"
        v-model="inputText"
        @input="inputChange"
        @keydown="inputKeyDown"
      >
      <div class="topCross">
        <img
          class="topCrossImgNormal"
          :class="{ typed: isClick, }"
          src="/delete_icon_top search_field_normal.svg"
          @click="crossClick"
        >
        <img
          class="topCrossImgClicked"
          :class="{ typed: isClick, }"
          src="/delete_icon_top search_field_clicked.svg"
        >
      </div>
      <div class="topMagnifier">
        <img
          class="topMagnifierImgNormal"
          :class="{ typed: isClick, }"
          src="/search_icon_top_search_field_normal.svg"
          @click="magnifierClick"
        >
        <img
          class="topMagnifierImgClicked"
          :class="{ typed: isClick, }"
          src="/search_icon_top_search_field_clicked.svg"
        >
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
.topSearchContainer {
  width: 1890px;
  height: 120px;
  box-shadow: 0 10px 14px 1px rgba(0, 0, 0, 0.25);
  background-color: #ffffff;
}

.topSearchContainer.typed {
  width: 1890px;
  height: auto;
  box-shadow: 0 10px 14px 1px rgba(0, 0, 0, 0.25);
  background-color: #ffffff;
}

.topSearchBarContainer {
  width: 1410px;
  height: 85px;
  display: flex;
  margin-left: auto;
  margin-right: auto;
}

.topSearchBarInput {
  width: 1300px;
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
  padding: 13px 0 14px 25px;
  outline: none;
}

::placeholder {
  color: #e6e6e6;
}

.topSearchBarInput.typed {
  color: #5f5f5f;
}

.topCross {
  border: none;
  border-bottom: solid 1px #d2af2c;
  cursor: pointer;
}

.topCrossImgNormal {
  padding: 29px 20px 0 20px;
}

.topCrossImgNormal.typed {
  display: none;
}

.topCrossImgClicked {
  padding: 29px 20px 0 20px;
}

.topCrossImgClicked:not(.typed) {
  display: none;
}

.topMagnifier {
  width: 72px;
  border: none;
  border-bottom: solid 1px #d2af2c;
  cursor: pointer;
}

.topMagnifierImgNormal {
  padding: 25px 0 0 25px;
}

.topMagnifierImgNormal.typed {
  display: none;
}

.topMagnifierImgClicked {
  padding: 25px 20px 0 20px;
}

.topMagnifierImgClicked:not(.typed) {
  display: none;
}

.topAutocompleteContainer {
  width: 1410px;
  margin-left: auto;
  margin-right: auto;
  background-color: #ffffff;
  padding: 30px 0 65px;
}

.topAutocompleteContainer:not(.typed) {
  display: none;
}

.topAutoCompListItem {
  padding-top: 15px;
  padding-left: 25px;
  padding-bottom: 15px;
  font-family: Muli;
  font-size: 28px;
  font-weight: normal;
  font-style: normal;
  font-stretch: normal;
  line-height: normal;
  letter-spacing: normal;
  color: #5f5f5f;
  cursor: pointer;
}

.topAutoCompListItem.selected {
  background: #e6e6e6;
  cursor: pointer;
}
</style>