<template>
  <div class="single-movie">
    <Loading v-if="$fetchState.pending" />

    <div v-else class="container">
      <NuxtLink class="button" :to="{ name: 'index'}">
        Back
      </NuxtLink>
      <div class="movie-info">
        <div class="movie-img">
          <img :src="`https://image.tmdb.org/t/p/w500${movie.poster_path}`" :alt="movie.title">
        </div>
        <div class="movie-content">
          <h1>Title: {{ movie.title }}</h1>
          <p class="movie-fact tagline">
            <span>Tagline:</span>" {{ movie.tagline }}"
          </p>
          <p class="movie-fact">
            <span>Released:</span> {{ releaseDateHandler (movie.release_date) }}
          </p>
          <p class="movie-fact">
            <span>Duration:</span> {{ movie.runtime }} minutes
          </p>
          <p class="movie-fact">
            <span>Revenue:</span> {{ revenueHandler(movie.revenue) }}
          </p>
          <p class="movie-fact">
            <span>Overview:</span>{{ movie.overview }}
          </p>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import axios from 'axios'
import Loading from '@/components/Loading.vue'

export default {
  name: 'SingleMovie',
  components: {
    Loading
  },

  data () {
    return {
      movie: {}
    }
  },
  async fetch () {
    await this.getSingleMovie()
  },
  fetchDelay: 1000,

  head () {
    return {
      title: this.movie.title
    }
  },

  methods: {
    async getSingleMovie () {
      const data = axios.get(
        `https://api.themoviedb.org/3/movie/${this.$route.params.movieID}?api_key=37ed43a4f8eaa2abd75f9283692947bc&language=en-US`
      )
      const result = await data
      this.movie = result.data
    },
    releaseDateHandler (movieReleaseDate) {
      return new Date(movieReleaseDate).toLocaleString('en-us', {
        month: 'long',
        day: 'numeric',
        year: 'numeric'
      })
    },
    revenueHandler (movieRevenue) {
      movieRevenue.toLocaleString('en-us', {
        style: 'currency',
        currency: 'USD'
      })
    }
  }
}
</script>

<style lang="scss">
.single-movie {
  color: #fff;
  min-height: 100vh;
  display: flex;
  flex-direction: column;
  justify-content: center;
  padding: 32px 16px;

  .button {
    align-self: flex-start;
    margin-bottom: 32px;
  }

  .movie-info {
    display: flex;
    flex-direction: column;
    align-items: center;
    gap: 32px;
    color: #fff;
    @media (min-width: 800px) {
      flex-direction: row;
      align-items: flex-start;
    }
    .movie-img {
      img {
        max-height: 500px;
        width: 100%;

        @media (min-width: 800px) {
          max-height: 700px;
          width: initial;
        }
      }
    }

    .movie-content {
      h1 {
        font-size: 56px;
        font-weight: 400;
      }

      .movie-fact {
        margin-top: 12px;
        font-size: 20px;
        line-height: 1.5;

        span {
          font-weight: 600;
          text-decoration: underline;
        }
      }

      .tagline {
        font-style: italic;
        span {
          font-style: normal;
        }
      }
    }
  }
}
</style>
