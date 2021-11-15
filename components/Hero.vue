<template>
  <div class="hero">
    <div class="text-container">
      <div class="text">
        <h1><span>Now</span>Watching</h1>
        <a href="#movie-grid" class="button">View Movies</a>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'Hero',
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