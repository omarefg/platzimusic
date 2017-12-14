<template lang="pug">
  #app
    img(src='dist/logo.png')
    h1 {{ msg }}
    select(v-model="selectedCountry")
      option(v-for="country in countries" v-bind:value="country.value") {{country.name}}
    spinner(v-show="loading")
    ul
      artist(v-for="artist in artists" v-bind:artist="artist" v-bind:key="artist.mbid")
</template>

<script>
import getArtists from './api'
import Artist from './components/Artist.vue'
import Spinner from './components/Spinner.vue'

export default {
  name: 'app',
  data () {
    return {
      msg: 'PlatziMusic',
      artists: [],
      countries: [
        {name: 'Colombia', value: 'colombia'},
        {name: 'Venezuela', value: 'venezuela'},
        {name: 'Peru', value: 'peru'},
      ],
      selectedCountry :'venezuela',
      loading: true,
    }
  },
  components: {
    Artist,
    Spinner,
  },
  methods:{
    refreshArtists(){
      const self = this
      this.loading = true
      this.artists = []
      getArtists(this.selectedCountry)
        .then(function(artists){
          self.artists = artists
          self.loading = false
        })
    }
  },
  //la siguiente función hace referencia al momento en el que es creado o llamado el módulo, puede ser created o mounted, usaré mounted
  mounted(){
    return this.refreshArtists()
  },
  watch: {
    selectedCountry: function(){
        return this.refreshArtists()
    },
  }
}
</script>

<style lang="stylus">
  #app
    font-family 'Avenir', Helvetica, Arial, sans-serif
    -webkit-font-smoothing antialiased
    -moz-osx-font-smoothing grayscale
    text-align center
    color #2c3e50
    margin-top 60px
  h1, h2
    font-weight normal
  ul
    list-style-type none
    padding 0
  li
    display inline-block
    margin 0 10px
    padding-bottom 55px
    border-bottom 1px dotted #42b983
  a
    color #42b983
    text-decoration none
</style>
