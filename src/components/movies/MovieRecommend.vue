<template>
  <div class="container">
    <div class="my-3">
      <div class="section__item">
        <div href="#0" class="sm-link sm-link_padding-all sm-link6">
          <h3 style="color: #E08119;" class="my-4">💖 {{username}}고객님의 맞춤영화 💖</h3>
        </div>
      </div>
    </div>

    <div v-if="choice.poster_url">
      <a href data-toggle="modal" :data-target="`#gg-${choice.id}`">
        <img class="card-img" :src="choice.poster_url" :alt="choice.title" />
      </a>
      <h2 class="my-1">👍 {{choice.title}} 👍</h2>
      <RecommendModal :choice="choice" />
    </div>
    <div v-else>
      <div class="jumbotron jumbotron-fluid my-5 bg-danger text-light" id="img">
        <div class="container">
          <h1 class="display-4">🤷 리뷰를 작성해주세요 🤷‍♂</h1>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import jwtDecode from "jwt-decode";
import RecommendModal from "./RecommendModal.vue";
import axios from "axios";

export default {
  name: "MovieRecommend",
  data() {
    return {
      reviews: [],
      fgenre: [],
      username: "",
      genremovie: [],
      fgenreid: [],
      choice: []
    };
  },
  components: {
    RecommendModal
  },
  methods: {
    getReviews() {
      this.$session.start();
      const token = this.$session.get("jwt");
      const decodedToken = jwtDecode(token);
      const user_id = decodedToken.user_id;
      const requestHeader = {
        headers: {
          Authorization: "JWT " + token
        }
      };
      axios
        .get(`http://localhost:8000/api/v1/accounts/${user_id}/`, requestHeader)
        .then(response => {
          this.username = response.data.username;
          this.reviews = response.data.review_set;
          let maxscore = 0;
          if (this.reviews.length > 0) {
            for (let review of this.reviews) {
              if (review.score === maxscore) {
                this.fgenre.push(review.movie.movie_genres);
              } else if (review.score > maxscore) {
                this.fgenre = review.movie.movie_genres;
                maxscore = review.score;
              }
            }
            for (let genre of this.fgenre) {
              this.fgenreid.push(genre.id);
            }
          }
        })
        .catch(error => {
          console.log(error);
        });
    },
    getGenres() {
      this.$session.start();
      const token = this.$session.get("jwt");
      const requestHeader = {
        headers: {
          Authorization: "JWT " + token
        }
      };
      axios
        .get(`http://localhost:8000/api/v1/movies/genres/`, requestHeader)
        .then(response => {
          this.genremovie = response.data;
          if (this.fgenreid.length > 0) {
            for (let gid of this.fgenreid) {
              for (let idx = 0; idx <= 27; idx++) {
                if (gid === idx) {
                  let maxvalue = 0;
                  for (let temp of response.data[idx - 1].movies) {
                    if (temp.score === maxvalue) {
                      this.choice.push(temp);
                    } else if (temp.score > maxvalue) {
                      this.choice = temp;
                      maxvalue = temp.score;
                    }
                  }
                }
              }
            }
          }
        })
        .catch(error => {
          console.log(error);
        });
    }
  },
  mounted: function() {
    this.getReviews();
    this.getGenres();
  }
};
</script>

<style>
select {
  display: block;
  width: 50% !important;
  margin: 2rem auto !important;
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