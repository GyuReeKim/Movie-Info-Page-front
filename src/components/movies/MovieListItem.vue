<template>
  <div>
    <a href data-toggle="modal" v-bind:data-target="`#gg-${movie.id}`">
      <div id="img" class="test">
        <img class="movie--poster my-3" v-bind:src="movie.poster_url" v-bind:alt="movie.title" />
      </div>
    </a>
    <h4>{{movie.title}}</h4>

    <MovieListItemModal :movie="movie" :genres="genres" />
  </div>
</template>

<script>
import MovieListItemModal from "./MovieListItemModal.vue";

export default {
  name: "MovieListItem",

  components: {
    MovieListItemModal
  },

  props: {
    movie: Object,
    genres: Array
  },

  data() {
    return {};
  }
};
</script>

<style>
.movie--poster {
  width: 200px;
}
@keyframes sparkle {
  from {
    background-position: 0% 100%;
  }
  to {
    background-position: 200% 100%;
  }
}
.test {
  background: #f3e0be;
  display: inline-block;
  padding: 1em;
  font-family: Helvetica Neue;
  border-radius: 4px;
  position: relative;
}
.test:before {
  animation: sparkle 6s infinite linear;
  /* background: linear-gradient(-60deg, #7fefbd 0%, #fff689 11%, #ec0b43 22%, #7fefbd 33%, #fff689 44%, #ec0b43 55%, #7fefbd 66%, #fff689 77%, #ec0b43 88%, #7fefbd 100%); */
  background: linear-gradient(
    -60deg,
    #f7cac9 0%,
    #f7786b 11%,
    #91a8d0 22%,
    #98ddde 33%,
    #f7cac9 44%,
    #f7786b 55%,
    #91a8d0 66%,
    #98ddde 77%,
    #f7cac9 88%,
    #f7786b 100%
  );
  /* background: linear-gradient(-60deg, #77212E 0%, #F5D6C6 11%, #FA9A85 22%, #5A3E36 33%, #CE5B78 44%, #935529 55%, #E08119 66%, #2A4B7C 77%, #577284 88%, #F96714 100%); */
  background-size: 300% 100%;
  content: "";
  position: absolute;
  left: 0;
  top: 0;
  width: 100%;
  height: 100%;
  border-radius: 4px;
  filter: blur(3px);
  z-index: -1;
  transform: scale(0.95) translateY(3px);
}
</style>
