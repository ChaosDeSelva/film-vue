<template>
  <div class="film-view" v-if="film">
    <div class="film-container">
      <div class="film-details">
        <h1>{{film.Title}}</h1>
        <p>{{film.Plot}}</p>

        <b>Actors</b>
        <p>{{film.Actors}}</p>

        <b>Directors</b>
        <p>{{film.Director}}</p>

        <b>Writers</b>
        <p>{{film.Writer}}</p>

        <b>Awards</b>
        <p>{{film.Awards}}</p>

        <b>Website</b>
        <p><a :href="film.Website" target="_blank">{{film.Website}}</a></p>

        <b>IMDB Score</b>
        <p>{{film.imdbRating}} ( {{film.imdbVotes}} Votes )</p>

        <b>Ratings</b>
        <div v-for="rating in film.Ratings">
          <p>{{rating.Source}} {{rating.Value}}</p>
        </div>
      </div>
      <div class="film-poster" align="center">
        <img :src="film.Poster" width="300px"/>
      </div>
    </div>
  </div>
</template>
<script>
import axios from 'axios';

export default {
  name: 'MovieDetails',
  props: {
    activeFilmId: String
  },
  data () {
    return {
      film: null,
    }
  },
  methods: {
    getFilm(activeFilmId) {
      axios.get(process.env.VUE_APP_APIURL,
      {
        params: {
            apikey: process.env.VUE_APP_APIKEY,
            i: activeFilmId
        }
      })
      .then(response => {
        if (response.data.Response === 'False') {
          alert(response.Error);
          return;
        }
        this.film = response.data;
      });
    }
  },
  watch: {
    activeFilmId(val) {
      this.getFilm(val);
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="scss">
.film-view {
  .film-container {
    display: flex;
    flex-direction: row;
  }

  .film-details {
    flex-grow: 4;
    text-align: left;
    padding-left: 25px;
  }

  .film-poster {
    flex-grow: 1;
  }
}
</style>
