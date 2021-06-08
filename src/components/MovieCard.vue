<template>
  <div class="details">
    <br />
    <div class="movie-image">
      <img :src="movie.poster" alt="Movie Poster" />
    </div>
    <h1>{{ movie.title }}</h1>
    <br />
    <h3>{{ movie.overview }}</h3>
    <p>Genres: {{ movie.genres }}</p>
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
</style>