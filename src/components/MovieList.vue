<template>
  <div class="movie-list" v-if="omdbResults.length > 0">
    <ul>
     <li v-for="film in omdbResults">
       <div class="film-container">
         <div class="film-wrapper">
           <div class="film-poster">
              <img :src="film.Poster" width="100px" />
           </div>
           <div class="film-details">
             <div>{{film.Title}}</div>
             <div>Year: {{film.Year}}</div>
             <div>Type: {{film.Type}}</div>
             <div>imdbID: {{film.imdbID}}</div>
             <br/>
             <button @click="onViewFilm(film.imdbID)">View Details</button>
           </div>
         </div>
       </div>
     </li>
    </ul>

    <div align="center">
      <button :disabled="pageIndex <= 1" @click="prevPage">Previous</button>
      <div class="button-divider"></div>
      <button :disabled="pageIndexError" @click="nextPage">Next</button>
    </div>
    <br/><br/>
  </div>
</template>

<script>
import axios from 'axios';

export default {
  name: 'MovieList',
  props: {
    searchValue: String
  },
  data () {
    return {
      omdbResults: [],
      pageIndex: 1,
      pageIndexError: false
    }
  },
  methods: {
    onViewFilm(id) {
      this.$emit('onView', id);
    },

    prevPage() {
      this.pageIndex--;
      this.getFilms(this.searchValue, this.pageIndex);

      if(this.pageIndexError) {
        this.pageIndexError = false;
      }
    },

    nextPage() {
      this.pageIndex++;
      this.getFilms(this.searchValue, this.pageIndex);
    },

    getFilms(searchValue) {
      axios.get(process.env.VUE_APP_APIURL,
      {
        params: {
            apikey: process.env.VUE_APP_APIKEY,
            page: this.pageIndex,
            s: searchValue
        }
      })
      .then(response => {
        if (response.data.Response === 'False') {
          alert(response.Error);
          return;
        }
        this.omdbResults = response.data.Search;
      });
    }
  },
  watch: {
    searchValue(val) {
      this.pageIndex = 1;
      this.getFilms(val);
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="scss">
.movie-list {
  ul {
    list-style-type: none;
  }

  .film-container {
    display: flex;
    flex-direction: row;
    width: 500px;
    padding-bottom: 20px;

    .film-wrapper {
      display: flex;
      width: 500px;
      border-bottom: 2px black solid;
    }

    .film-poster {
      width: 100px;
      min-height: 150px;
    }

    .film-details {
      flex-grow: 2;
      padding-left: 5px;
    }
  }

  .button-divider {
    width:15px;
    height:auto;
    display:inline-block;
  }
}
</style>
