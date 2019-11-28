<template>
  <div>
    <div class>
      <div class="section__item">
        <div href="#0" class="sm-link sm-link_padding-all sm-link6">
          <h3 class="sm-link__label" style="color: #E08119;">🎬 장르별 영화 🎬</h3>
        </div>
      </div>
    </div>
    <select class="form-control bg-dark text-light" v-model="selectedGenreId">
      <option id="orange" value>전체보기</option>
      <option id="orange" v-for="genre in genres" :key="genre.id" :value="genre.id">{{genre.name}}</option>
    </select>

    <div class="row">
      <div
        class="col-12 col-md-6 col-lg-3 my-3"
        v-for="movie in onselectedGenreMovies"
        :key="movie.id"
      >
        <MovieListItem :movie="movie" :genres="genres" />
      </div>
    </div>
  </div>
</template>

<script>
import MovieListItem from "./MovieListItem.vue";

export default {
  name: "MovieList",
  components: {
    MovieListItem
  },
  data() {
    return {
      selectedGenreId: ""
    };
  },
  props: ["genres", "movies"],
  computed: {
    onselectedGenreMovies: function() {
      if (this.selectedGenreId === "") {
        return this.movies;
      } else {
        return this.genres[this.selectedGenreId - 1].movies;
      }
    }
  }
};
</script>

<style>
select {
  display: block;
  width: 50% !important;
  margin: 2rem auto !important;
}
#orange {
  color: orange;
}
/* * core styles */
.sm-link {
  --uismLinkDisplay: var(--smLinkDisplay, inline-flex);
  --uismLinkTextColor: var(--smLinkTextColor);
  --uismLinkTextColorHover: var(--smLinkTextColorHover);
  display: var(--uismLinkDisplay);
  color: var(--uismLinkTextColor);
  position: relative;
  overflow: hidden;
}
div.sm-link {
  text-decoration: none;
}
.sm-link__label {
  display: block;
}
/* sm-link_padding-all */
.sm-link_padding-all {
  --uismLinkLineWeight: var(--smLinkLineWeight, 2px);
  --uismLinkLineColor: var(--smLinkLineColor, #000);
  --uismLinkPadding: var(--smLinkPadding, 5px);
  padding: var(--uismLinkPadding);
}
.sm-link_padding-all::before,
.sm-link_padding-all::after {
  width: 100%;
  height: var(--uismLinkLineWeight);
  left: 0;
}
.sm-link_padding-all::before {
  top: 0;
}
.sm-link_padding-all::after {
  bottom: 0;
}
.sm-link_padding-all .sm-link__label::before,
.sm-link_padding-all .sm-link__label::after {
  width: var(--uismLinkLineWeight);
  height: 100%;
  top: 0;
}
.sm-link_padding-all .sm-link__label::before {
  left: 0;
}
.sm-link_padding-all .sm-link__label::after {
  right: 0;
}
.sm-link_padding-all::before,
.sm-link_padding-all::after,
.sm-link_padding-all .sm-link__label::before,
.sm-link_padding-all .sm-link__label::after {
  content: "";
  background-color: var(--uismLinkLineColor);
  position: absolute;
  opacity: 0;
  will-change: transform, opacity;
  transition-property: transform, opacity;
}
.sm-link_padding-all:hover::before,
.sm-link_padding-all:hover::after,
.sm-link_padding-all:hover .sm-link__label::before,
.sm-link_padding-all:hover .sm-link__label::after {
  opacity: 1;
}
/* sm-link_padding-bottom */
.sm-link_padding-bottom {
  --uismLinkLineWeight: var(--smLinkLineWeight, 2px);
  --uismLinkLineColor: var(--smLinkLineColor, #000);
  padding-bottom: var(--uismLinkLineWeight);
  position: relative;
}
.sm-link_padding-bottom::after {
  content: "";
  width: 100%;
  height: var(--uismLinkLineWeight);
  background-color: var(--uismLinkLineColor);
  position: absolute;
  left: 0;
  bottom: 0;
}
/* sm-link_bg */
.sm-link_bg {
  --uismLinkLineColor: var(--smLinkLineColor, #000);
  --uismLinkTextColorHover: var(--smLinkTextColorHover, #fff);
  --uismLinkPadding: var(--smLinkPadding, 5px);
  padding: var(--uismLinkPadding);
  transition: color 0.3s ease-out;
}
.sm-link_bg::before,
.sm-link_bg::after {
  content: "";
  background-color: var(--uismLinkLineColor);
  opacity: 0;
  position: absolute;
  transition: transform 0.2s ease-out, opacity 0.2s ease-out 0.03s;
}
.sm-link_bg .sm-link__label {
  position: relative;
  z-index: 2;
}
.sm-link_bg:hover::before,
.sm-link_bg:hover::after {
  opacity: 1;
  transition-duration: 0.35s, 0.35s;
  transition-delay: 0s, 0s;
}
.sm-link_bg:hover {
  color: var(--uismLinkTextColorHover);
}
/* sm-link_text */
.sm-link_text::before {
  content: attr(data-sm-link-text);
  color: var(--uismLinkTextColorHover);
  position: absolute;
}
.sm-link_text::before,
.sm-link_text .sm-link__label {
  transition-property: transform;
  transition-timing-function: cubic-bezier(0.86, 0.6, 0.08, 1.01);
  transition-duration: 0.3s;
}
.sm-link_text:hover::before,
.sm-link_text:hover .sm-link__label {
  transition-duration: 0.4s;
}
/* effect 5 */
.sm-link5::before,
.sm-link5::after,
.sm-link5 .sm-link__label::before,
.sm-link5 .sm-link__label::after {
  transition-timing-function: ease-out;
  transition-duration: 0.2s, 0.15s;
  transition-delay: 0s, 0.15s;
}
.sm-link5::before {
  transform: translate3d(-100%, 0, 0);
}
.sm-link5::after {
  transform: translate3d(100%, 0, 0);
}
.sm-link5 .sm-link__label::before {
  transform: translate3d(0, 100%, 0);
}
.sm-link5 .sm-link__label::after {
  transform: translate3d(0, -100%, 0);
}
.sm-link5:hover::before,
.sm-link5:hover::after,
.sm-link5:hover .sm-link__label::before,
.sm-link5:hover .sm-link__label::after {
  transform: translate3d(0, 0, 0);
  transition-delay: 0s;
}
/* effect 6 */
.sm-link6::before,
.sm-link6::after,
.sm-link6 .sm-link__label::before,
.sm-link6 .sm-link__label::after {
  transition-duration: 0.2s;
  transition-timing-function: ease-out;
}
.sm-link6::before,
.sm-link6::after {
  width: 100%;
  height: var(--uismLinkLineWeight);
  left: 0;
}
.sm-link6 .sm-link__label::before,
.sm-link6 .sm-link__label::after {
  width: var(--uismLinkLineWeight);
  height: 100%;
  top: 0;
}
.sm-link6::before {
  top: 0;
  transform: translate3d(-105%, 0, 0);
}
.sm-link6::after {
  bottom: 0;
  transform: translate3d(105%, 0, 0);
}
.sm-link6 .sm-link__label::before {
  left: 0;
  transform: translate3d(0, 105%, 0);
}
.sm-link6 .sm-link__label::after {
  right: 0;
  transform: translate3d(0, -105%, 0);
}
.sm-link6:hover::before,
.sm-link6:hover::after,
.sm-link6:hover .sm-link__label::before,
.sm-link6:hover .sm-link__label::after {
  transform: translate3d(0, 0, 0);
}
.sm-link6:hover::before {
  transition-delay: 0s;
}
.sm-link6 .sm-link__label::after,
.sm-link6:hover::after {
  transition-delay: 0.25s;
}
.sm-link6::after,
.sm-link6:hover .sm-link__label::after {
  transition-delay: 0.15s;
}
.sm-link6::before,
.sm-link6:hover .sm-link__label::before {
  transition-delay: 0.35s;
}
/* SETTINGS */
.sm-link {
  --smLinkPadding: 10px 15px;
  --smLinkLineWeight: 5px;
  --smLinkLineColor: #e08119;
  --smLinkTextColor: #243aab;
  --smLinkTextColorHover: #1b255a;
}
.sm-link_bg {
  --smLinkTextColorHover: #fff;
}
/* ===== DEMO ===== */
body {
  font-family: -apple-system, BlinkMacSystemFont, "Roboto", "Open Sans",
    "Helvetica Neue", "Segoe UI", "Arial", sans-serif;
  margin: 0;
}
.section__item {
  margin: auto;
}
</style>