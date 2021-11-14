<template>
  <!-- Loading -->
  <Loading v-if="$fetchState.pending" />

  <!-- Movie Info -->
  <div v-else>
    <NuxtLink class="button" :to="{ name: 'index' }"> Back </NuxtLink>
    <div class="movie-background">
      <img
        :src="`https://image.tmdb.org/t/p/original/${movie.backdrop_path}`"
        alt=""
        class="mb-5 img-fluid w-100"
      />
    </div>
    <div class="container single-movie">
      <div class="row">
        <div class="col-3 pt-2">
          <div style="border-radius: 15px; overflow: hidden">
            <img
              :src="`https://image.tmdb.org/t/p/w400/${movie.poster_path}`"
              alt=""
              class="img-fluid"
            />
          </div>
        </div>
        <div class="col-9 movie-content">
          <h1 class="title">{{ movie.title }}</h1>
          <p class="movie-fact tagline">
            <span>Tagline:</span>
            "{{ movie.tagline }}"
          </p>
          <p class="movie-fact">
            <span>Released:</span>
            {{
              new Date(movie.release_date).toLocaleString('fr-fr', {
                month: 'long',
                day: 'numeric',
                year: 'numeric',
              })
            }}
          </p>
          <p class="movie-fact">
            <span>Duration:</span> {{ movie.runtime }} minutes
          </p>
          <p class="movie-fact">
            <span>Revenue:</span>
            {{
              movie.revenue.toLocaleString('en-us', {
                style: 'currency',
                currency: 'USD',
              })
            }}
          </p>
          <p class="movie-fact"><span>Overview:</span> {{ movie.overview }}</p>
          <div>
            <span>Genres:</span>
            <p v-for="genre in movie.genres" :key="genre.name" class="d-inline">
              {{ genre.name }}
            </p>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import axios from 'axios'

export default {
  name: 'SingleMovie',

  data() {
    return {
      movie: '',
    }
  },

  async fetch() {
    await this.getSingleMovie()
  },

  // delay for fetching
  fetchDelay: 1000,

  //! SEO OTIMISATION
  head() {
    return {
      title: this.movie.title,
      meta: [
        // hid is used as unique identifier. Do not use `vmid` for it as it will not work
        {
          // META INFO
          hid: 'keywords',
          name: 'keywords',
          content: this.movie.title,
        },
      ],
    }
  },
  methods: {
    async getSingleMovie() {
      const data = axios.get(
        `https://api.themoviedb.org/3/movie/${this.$route.params.id}?api_key=37ed43a4f8eaa2abd75f9283692947bc&language=en-US`
      )
      const result = await data
      this.movie = result.data
    },
  },
}
</script>

<style lang="scss" scoped>
.title {
  font-size: 60px;
  font-weight: 400;
  color: #c92502;
}
span {
  text-decoration: underline;
}

.movie-background {
  min-height: 100px;
  max-height: 40vh;
  overflow: hidden;
  margin-left: -6px;
  margin-top: -9px;
}
.button {
  align-self: flex-start;
  // margin-bottom: 32px;
  position: absolute;
  top: 32px;
  left: 32px;
}
.single-movie {
  color: #fff;
  display: flex;
  flex-direction: column;
  justify-content: center;
  padding: 32px 16px;

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
