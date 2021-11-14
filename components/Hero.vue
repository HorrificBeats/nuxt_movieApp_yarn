<template>
  <div class="hero">
    <div class="text-container">
      <div class="text">
        <span class="mini-heading">Now Streaming</span>
        <h1><span>Now</span> Streaming</h1>
        <a href="#movie-grid" class="button">View Movies</a>
      </div>
    </div>
  </div>
</template>

<script>
import axios from 'axios'
export default {
  name: 'Hero',
  data() {
    return {
      // Initialising reactive variables
      trendingMovie: '',
    }
  },
  async fetch() {
    await this.getTrendingMovie
  },
  methods: {
    async getTrendingMovie() {
      // Requesting data /w axios
      const data = axios.get(
        `https://api.themoviedb.org/3/trending/movie/day?api_key=adced3e6aeb79e0749d5c328257e0461`
      )
      // Assigning PROMISE reponsive to `result`
      const result = await data

      // Parsing `results`
      result.data.results.forEach((movie) => {
        this.movies.push(movie)
      })
      // eslint-disable-next-line no-console
      console.log(result)
    },
  },
}
</script>

<style lang="scss" scoped>
.hero {
  height: 400px;
  position: relative;
  background-image: url('../assets/imgs/movieHero.jpg');
  background-position: center;
  background-size: cover;
  @media (min-width: 750px) {
    height: 500px;
  }
  &::after {
    content: '';
    position: absolute;
    top: 0;
    left: 0;
    height: 100%;
    width: 100%;
    background-color: rgba(0, 0, 0, 0.6);
  }
  img {
    width: 100%;
    height: 100%;
    object-fit: cover;
  }
  .text-container {
    z-index: 99;
    position: absolute;
    top: 0;
    margin: 0;
    width: 100%;
    height: 100%;
    display: flex;
    flex-direction: row;
    justify-content: flex-start;
    align-items: center;
    .text {
      padding: 0 16px;
      width: 100%;
      max-width: 1400px;
      margin: 0 auto;
    }
    .mini-heading {
      font-weight: 600;
      font-size: 18px;
      text-transform: uppercase;
      color: var(--color-primary);
      margin-bottom: 8px;
      @media (min-width: 750px) {
        font-size: 22px;
      }
    }
    h1 {
      color: #fff;
      font-size: 64px;
      font-weight: 200;
      margin-bottom: 8px;
      @media (min-width: 750px) {
        font-size: 84px;
      }
      span {
        font-weight: 500;
      }
    }
    .button {
      font-size: 20px;
      align-self: flex-start;
    }
  }
}
</style>