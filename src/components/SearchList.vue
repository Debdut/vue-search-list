<template lang="pug">
  .search-list(
    @keyup="toggle"
  )
    input(v-model="search" ref="search-list-input"
    @keyup="change")
    ul.list
      li.list-item(v-for="(item, index) in searchResults" :class="{gray: selected === index}"
      @click="select(index)") {{ item }}
</template>

<script>

const searchAlgorithms = {
  'block-first': (search) => (item, index, list) => (item.indexOf(search) === 0),
  'block-anywhere': (search) => (item, index, list) => (item.indexOf(search) > -1)
}

export default {
  props: {
    data: Array,
    algorithm: String
  },
  data () {
    return {
      search: '',
      selected: 0
    }
  },
  computed: {
    searchResults () {
      return this.data.filter(searchAlgorithms[this.algorithm || 'block-first'](this.search))
    }
  },
  methods: {
    broadcast (event, payload) {
      if (payload) {
        this.$emit(event, payload)
      } else {
        this.$emit(event)
      }
    },
    select (index) {
      this.selected = index
      this.broadcast('update-selected', this.searchResults[this.selected])
      this.$nextTick(() => {
        this.$refs['search-list-input'].focus()
      })
    },
    change (event) {
      if (['ArrowUp', 'ArrowDown'].indexOf(event.key) === -1) {
        this.selected = 0
        this.broadcast('update-selected', this.searchResults[this.selected])
      }
    },
    toggle (event) {
      if (event.key === 'ArrowDown' && this.selected < this.searchResults.length - 1) {
        this.selected += 1
        this.broadcast('update-selected', this.searchResults[this.selected])
      } else if (event.key === 'ArrowUp' && this.selected > 0) {
        this.selected -= 1
        this.broadcast('update-selected', this.searchResults[this.selected])        
      }
    }
  }
}
</script>

<style lang="sass" scoped>
  .list
    list-style-type: none
    -webkit-padding-start: 20px
  .gray
    background: #eee
  input
    width: 100%
</style>
