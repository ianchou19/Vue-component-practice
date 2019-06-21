<template>
  <div class="searchContainer">
    <div class="searchBarContainer">
      <input
        :class="{ typed: isType, }"
        class="SearchBarInput"
        type="text"
        :placeholder="placeholder"
        @input="inputChange"
        @keydown="inputKeyDown"
      >
      <div class="cross">
        <img
          :class="{ typed: isClick, }"
          class="crossImgNormal"
          src="/delete_icon_search_field_normal.svg"
          @click="cleanInput"
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
        v-for="amd_item in autocomplete_mock_data"
        class="autoCompListItem"
        :id="amd_item.id"
        style="list-style-type:none;"
        @click="liClick"
      >
{{ amd_item.content }}
</li>
    </div>
  </div>
</template>

<script>
import jQuery from 'jquery'
const $ = jQuery

export default {
  props: {
    placeholder: {
      type: String,
      default: 'Search for brand, style...etc'
    }
  },
  data: function () {
    return {
      isType: false,
      isClick: false,
      defaultValue: '',
      autocomplete_mock_data: {
        fakeData_1: {
          id: 'item_1',
          content: 'apple'
        },
        fakeData_2: {
          id: 'item_2',
          content: 'apple farmer'
        },
        fakeData_3: {
          id: 'item_3',
          content: 'apple is delicious'
        },
        fakeData_4: {
          id: 'item_4',
          content: 'apple apple apple!'
        },
        fakeData_5: {
          id: 'item_5',
          content: 'apple can save your life'
        }
      }
    }
  },
  methods: {
    inputChange: function (item) {
      if (item.target.value !== '') {
        if (!this.isType) {
          this.isType = true
        }
      } else if (this.isType) {
          this.isType = false
        }
    },
    inputKeyDown: function (item) {
      if (item.key === 'ArrowDown') {
        this.moveSelection('down')
      } else if (item.key === 'ArrowUp') {
        this.moveSelection('up')
      } else if (item.key === 'Enter') {
        this.onSearch()
      }
    },
    liClick: function (item) {
      $('.autoCompListItem.selected').removeClass('selected')
      let selector = '#' + item.target.id
      $(selector).addClass('selected')
      $('.SearchBarInput')[0].value = item.target.innerText
      this.onSearch()
    },
    moveSelection: function (direction) {
      if (direction === 'up') {
        direction = -1
      } else if (direction === 'down') {
        direction = 1
      }

      if (!$('.autoCompListItem').hasClass('selected')) {
        var selector = '#item_1'
        $(selector).addClass('selected')
      } else {
        var selector = '#' + $('.autoCompListItem.selected').attr('id')
        $(selector).removeClass('selected')
        var selector =
          selector.slice(0, -1) +
          (parseInt(selector.slice(-1)) + direction).toString()
        $(selector).addClass('selected')
      }

      if ($(selector)[0] !== undefined) {
        $('.SearchBarInput').val($(selector)[0].innerHTML)
      }
    },
    onSearch: function () {
      console.log('on search!')
      this.isType = false
    },
    cleanInput: function (item) {
      $('.SearchBarInput')[0].value = ''
      this.isType = false
    },
    magnifierClick: function () {
      this.onSearch()
    }
  }
}
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
  width: 700px;
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
  height: 47px;
  border-top: solid 1px #d2af2c;
  border-right: solid 1px #d2af2c;
  border-bottom: solid 2px #d2af2c;
  cursor: pointer;
}

.crossImgNormal {
  padding: 14px 20px 0 20px;
}

.crossImgNormal.typed {
  display: none;
}

.crossImgClicked {
  padding: 14px 20px 0 20px;
}

.crossImgClicked:not(.typed) {
  display: none;
}

.magnifier {
  width: 62px;
  height: 47px;
  border-top: solid 1px #d2af2c;
  border-right: solid 1px #d2af2c;
  border-bottom: solid 2px #d2af2c;
  cursor: pointer;
}

.magnifierImgNormal {
  padding: 14px 20px 0 20px;
}

.magnifierImgNormal.typed {
  display: none;
}

.magnifierImgClicked {
  padding: 12px 20px 0 20px;
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
  background: #5f5f5f;
  cursor: pointer;
}
</style>
