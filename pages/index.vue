<template>
  <div ref="container" class="home">
    <!-- HERO -->
    <Hero />

    <!-- SEARCH -->
    <div id="movie-grid" class="container search">
      <input
        ref="search"
        v-model.lazy="searchInput"
        type="text"
        placeholder="Search"
        class="text-dark form-control border shadow-sm"
        @keyup.enter="$fetch"
        @keyup.esc="clearSearch"
      />
      <button v-if="searchInput !== ''" class="button" @click="clearSearch">
        Clear search
      </button>
    </div>

    <!-- LOADING -->
    <Loading v-if="$fetchState.pending" />

    <!-- MOVIES -->
    <div v-else class="container movies">
      <!-- SEARCHED MOVIES -->
      <div v-if="searchInput !== ''" class="movies-grid">
        <!-- Iterating throught the new variable, containg the results of our query -->
        <div
          v-for="(movie, index) in searchedMovies"
          :key="index"
          class="movie"
        >
          <div class="movie-img">
            <!-- Building the imagePath from 3 elements: baseUrl, size, filePath -->
            <img
              :src="`https://image.tmdb.org/t/p/w400/${movie.poster_path}`"
              alt=""
            />
            <p class="review">{{ movie.vote_average }}</p>
            <p class="overview">
              {{ movie.overview.slice(0, 100)
              }}<span v-if="movie.overview.length > 100">...</span>
            </p>
          </div>
          <div class="info">
            <p class="title">
              {{ movie.title.slice(0, 25)
              }}<span v-if="movie.title.length > 25">...</span>
            </p>
            <p class="release">
              Released:
              {{
                new Date(movie.release_date).toLocaleString('en-us', {
                  month: 'long',
                  day: 'numeric',
                  year: 'numeric',
                })
              }}
            </p>
            <NuxtLink
              class="button button-light"
              :to="{ name: 'movies-id', params: { id: movie.id } }"
            >
              Get more info
            </NuxtLink>
          </div>
        </div>
      </div>

      <!-- NOW STREAMING -->
      <div v-else id="movie-grid" class="movies-grid">
        <div v-for="(movie, index) in movies" :key="index" class="movie">
          <div class="movie-img">
            <!-- Building the imagePath from 3 elements: baseUrl, size, filePath -->
            <img
              :src="`https://image.tmdb.org/t/p/w400/${movie.poster_path}`"
              alt=""
            />
            <p class="review">{{ movie.vote_average }}</p>
            <p class="overview">
              {{ movie.overview.slice(0, 100)
              }}<span v-if="movie.overview.length > 100">...</span>
            </p>
          </div>
          <div class="info">
            <p class="title">
              {{ movie.title.slice(0, 25)
              }}<span v-if="movie.title.length > 25">...</span>
            </p>
            <p class="release">
              Released:
              {{
                new Date(movie.release_date).toLocaleString('en-us', {
                  month: 'long',
                  day: 'numeric',
                  year: 'numeric',
                })
              }}
            </p>
            <NuxtLink
              class="button button-light"
              :to="{ name: 'movies-id', params: { id: movie.id } }"
            >
              Get more info
            </NuxtLink>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import axios from 'axios'

export default {
  data() {
    return {
      // Initialising reactive variables
      movies: [],
      searchedMovies: [],
      searchInput: '',
      lastSearch: '',
      currentPage: `https://api.themoviedb.org/3/movie/now_playing?api_key=adced3e6aeb79e0749d5c328257e0461&language=en-US&page=1`,

      colors: ['system', 'light', 'dark', 'sepia'],
    }
  },

  // Calling our method using Nuxt's FETCH Cycle Hook
  async fetch() {
    if (this.searchInput === '') {
      await this.getMovies()
    }

    if (this.searchInput !== '') {
      await this.searchMovies()
    }
    if (this.searchInput !== this.lastSearch) {
      await this.searchMovies()
    }
  },

  // SEO OTIMISATION
  head() {
    return {
      title: 'Nuxt Movie App',
      meta: [
        // hid is used as unique identifier. Do not use `vmid` for it as it will not work
        {
          // META INFO
          hid: 'description',
          name: 'description',
          content: 'Search between all the streaming movies',
        },
        {
          // META INFO
          hid: 'keywords',
          name: 'keywords',
          content: 'movies, films, theaters, stream, streaming',
        },
      ],
    }
  },

  activated() {
    // Call fetch again if last fetch more than 30 sec ago
    if (this.$fetchState.timestamp <= Date.now() - 30000) {
      this.$fetch()
    }
  },

  mounted() {
    // Working DOM Event
    this.$refs.search.focus()

    // Scroll to bottom
    this.$refs.container.scrollTop = this.$refs.container.scrollHeight
  },

  fetchDelay: 1000,

  methods: {
    async getMovies() {
      // Requesting data /w axios
      const data = axios.get(this.currentPage)
      // Assigning PROMISE reponsive to `result`
      const result = await data

      // Parsing `results`
      result.data.results.forEach((movie) => {
        this.movies.push(movie)
      })
    },

    async searchMovies() {
      const data = axios.get(
        `https://api.themoviedb.org/3/search/movie?api_key=37ed43a4f8eaa2abd75f9283692947bc&language=en-US&page=1&query=${this.searchInput}`
      )
      const result = await data
      // this.searchedMovies = []
      result.data.results.forEach((movie) => {
        this.searchedMovies.push(movie)
      })
      this.lastSearch = this.searchInput
    },

    clearSearch() {
      this.searchInput = ''
      this.searchedMovies = []
    },
  },
}
</script>



<style lang="scss" scoped>
.home {
  .loading {
    padding-top: 120px;
    align-items: flex-start;
  }
  .search {
    display: flex;
    padding: 32px 16px;
    input {
      max-width: 350px;
      width: 100%;
      padding: 12px 6px;
      font-size: 14px;
      border: none;
      &:focus {
        outline: none;
      }
    }
    .button {
      margin-left: 1rem;
      &:hover {
        background: var(--color-secondary);
        color: var(--bg);
      }
    }
  }
  .movies {
    padding: 32px 16px;
    .movies-grid {
      display: grid;
      column-gap: 32px;
      row-gap: 64px;
      grid-template-columns: 1fr;
      @media (min-width: 500px) {
        grid-template-columns: repeat(2, 1fr);
      }
      @media (min-width: 750px) {
        grid-template-columns: repeat(2, 1fr);
      }
      @media (min-width: 1100px) {
        grid-template-columns: repeat(4, 1fr);
      }
      .movie {
        position: relative;
        display: flex;
        flex-direction: column;
        .movie-img {
          position: relative;
          overflow: hidden;
          border-radius: 15px;
          box-shadow: 0 1px 3px rgba(0, 0, 0, 0.12),
            0 1px 2px rgba(0, 0, 0, 0.24);
          transition: all 0.3s;
          &:hover {
            transform: translateY(-3%);
            transition: all 0.3s;

            .overview {
              transform: translateY(20%);
            }
            .release {
              color: var(--color-primary) !important;
            }
          }
          img {
            display: block;
            width: 100%;
            height: 100%;
          }
          .review {
            position: absolute;
            top: 0;
            left: 0;
            display: flex;
            justify-content: center;
            align-items: center;
            width: 40px;
            height: 40px;
            background-color: var(--color-primary);
            color: #fff;
            border-radius: 0 0 16px 0;
            box-shadow: 0 4px 6px -1px rgba(0, 0, 0, 0.1),
              0 2px 4px -1px rgba(0, 0, 0, 0.06);
          }
          .overview {
            line-height: 1.5;
            position: absolute;
            bottom: 0;
            background-color: var(--color-primary);
            padding: 12px;
            color: #fff;
            transform: translateY(120%);
            transition: 0.3s ease-in-out all;
            text-overflow: ellipsis;
            box-shadow: 0px 0 10px rgba(0, 0, 0, 0.8);
          }
        }
        .info {
          margin-top: auto;
          .title {
            margin-top: 8px;
            margin-bottom: 0;
            color: var(--color-primary);
            font-size: 20px;
          }
          .release {
            color: var(--color);
          }
          .button {
            margin-top: 8px;
          }
        }
      }
    }
  }
}
</style>
