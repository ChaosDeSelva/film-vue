<template>
  <div id="app">
    <Search msg="Search by movie title..." @onSubmit="handleSearchSubmit"/>
    <br/><br/>
    <div class="container">
      <div class="film-list-view">
        <MovieList :searchValue="searchValue" @onView="handleViewMovie"/>
      </div>
      <div class="film-detail-view">
        <MovieDetails :activeFilmId="activeFilmId" />
      </div>
    </div>
  </div>
</template>

<script>
import Search from './components/common/Search.vue'
import MovieList from './components/MovieList.vue'
import MovieDetails from './components/MovieDetails.vue'

export default {
  name: 'app',
  components: {
    Search,
    MovieList,
    MovieDetails
  },
  data () {
    return {
      searchValue: '',
      activeFilmId: ''
    }
  },
  methods: {
    handleSearchSubmit(value) {
      this.searchValue = value;
    },
    handleViewMovie(value) {
      this.activeFilmId = value;
    }
  }
}
</script>

<style lang="scss">
  #app {
    font-family: 'Avenir', Helvetica, Arial, sans-serif;
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
    text-align: center;
    color: #2c3e50;
    margin-top: 60px;
  }

  .container {
    display: flex;
    flex-direction: row;

    .film-detail-view {
      flex-grow: 2;
    }

    .film-list-view {
      flex-grow: 1;
    }
  }
</style>
