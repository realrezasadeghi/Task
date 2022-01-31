<template>
  <div class="auto-complete">
    <label for="search">search</label>
    <input
      id="search"
      v-model="search"
      type="text"
      class="search-auto-complete"
      name="search"
      @keyup.prevent="searchAutoComplete"
      @keyup.down="onArrowDown"
      @keyup.up="onArrowUp"
      @keypress.enter="onEnter"
    />
    <ul v-if="isShowListAutoComplete" class="list-auto-complete">
      <li
        v-for="(item, index) in listResult"
        :key="index"
        class="item-auto-complete"
        :class="{ active: index === arrowCounter }"
        @click="selectAutoComplete(item)"
      >
        {{ item }}
      </li>
    </ul>
  </div>
</template>

<script>
export default {
  name: 'AutoComplete',
  props: {
    listData: {
      type: Array,
      required: true,
    },
  },

  data() {
    return {
      search: '',
      listResult: [],
      isShowListAutoComplete: false,
      appId: 'c983d82f65c624bedf1c75daf764138c',
      arrowCounter: -1,
      timer: null,
      waitTime: 1000,
    }
  },
  watch: {
    search(newVal) {
      if (newVal === '' || newVal === ' ') {
        this.isShowListAutoComplete = false
        this.listResult = []
      }
    },
  },
  methods: {
    filterAutoComplete() {
      this.listResult = this.listData.filter((data) =>
        data.toLowerCase().includes(this.search.toLowerCase())
      )
    },
    searchAutoComplete() {      
      this.filterAutoComplete()
      if (this.listResult.length===0) {
        this.isShowListAutoComplete = true
      }else{
        this.isShowListAutoComplete = false
      }
      clearTimeout(this.timer)
      this.timer = setTimeout(() => {
        this.getInfoCity()
      }, this.waitTime)
    },
    selectAutoComplete(item) {
      this.search = item
      this.isShowListAutoComplete = false
      setTimeout(this.getInfoCity(), 1000)
    },
    onBlur() {
      this.isShowListAutoComplete = false
    },
    onArrowDown() {
      if (this.arrowCounter < this.listResult.length) {
        this.arrowCounter += 1
      }
    },
    onArrowUp() {
      if (this.arrowCounter > 0) {
        this.arrowCounter -= 1
      }
    },
    onEnter(event) {
      event.preventDefault()
      this.search = this.listResult[this.arrowCounter]
      this.arrowCounter = -1
      this.isShowListAutoComplete = false
      setTimeout(this.getInfoCity(), 1000)
    },
    async getInfoCity() {
      const { data } = await this.$axios.get(
        `/weather?q=${this.search}&appid=${this.appId}`
      )
      this.$emit('infoCity', data)
      this.search = ''
    },
  },
}
</script>

<style></style>
