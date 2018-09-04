<template lang="pug">
  #app
    search-list(:data="words" :algorithm="'block-anywhere'" @update-selected="word = $event")
    search-list(:data="words" :algorithm="'block-anywhere'" @update-selected="word = $event")
    h1 {{ word }}
</template>

<script>
import SearchList from './components/SearchList'

export default {
  name: 'app',
  components: {
    SearchList
  },
  data () {
    return {
      word: 'app',
      words: [
        'app',
        'apple',
        'appy',
        'apple juice',
        'man',
        'ma',
        'mango',
        'many'
      ]
    }
  }
}
</script>

<style lang="sass">
#app
  font-family: 'Avenir', Helvetica, Arial, sans-serif
  -webkit-font-smoothing: antialiased
  -moz-osx-font-smoothing: grayscale
  color: #2c3e50
  margin-top: 60px
  .search-list
    width: 200px
</style>
