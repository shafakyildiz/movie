<template>
  <div class="details">
    <br />
    <div class="movie-image">
      <img :src="movie.poster" alt="Movie Poster" />
    </div>
    <h1>{{ movie.title }}</h1>
    <h3 class="movie-overview">{{ movie.overview }}</h3>
    <br />
  </div>
</template>

<script>
export default {
  name: "MovieCard",
  props: {
    movie: Object,
  },
};
</script>

<style>
p,
h1,
h3,
.movie-image {
  padding: 5px;
  margin: 5px;
  display: flex;
  justify-content: center;
  text-align: center;
  align-items: center;
  color: rgb(211, 206, 198);
}

.movie-overview {
  width: 40%;
  display: flex;
  justify-content: center;
  align-items: center;
  margin: 0 auto;
}
</style>
