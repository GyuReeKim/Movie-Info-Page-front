<template>
  <div
    class="modal fade"
    tabindex="-1"
    role="dialog"
    v-bind:id="`gg-${movie.id}`"
    data-backdrop="static"
    data-keypress="false"
  >
    <div class="modal-dialog modal-lg" role="document">
      <div class="modal-content bg-dark text-white">
        <div class="modal-header">
          <h5 class="modal-title">🎬 {{movie.title}} ({{movie.title_en}})</h5>
          <button type="button" class="close text-white" data-dismiss="modal" aria-label="Close">
            <span aria-hidden="true">&times;</span>
          </button>
        </div>

        <div class="modal-body">
          <div id="img">
            <img
              class="movie--poster my-3 mx-auto"
              v-bind:src="movie.poster_url"
              v-bind:alt="movie.title"
              style="width:50%"
            />
          </div>
          <hr style="background-color:white" />
          <div>
            <p class="text-center">DETAIL</p>
            <div class="mb-1">평점 : {{movie.score}}</div>
            <div class="mb-1">등급 : {{movie.grade.name}}</div>
            <div class="mb-1">
              <span>장르 :</span>
              <span v-for="(genre, index) in movie.movie_genres" :key="genre.id">
                <span v-if="index !== movie.movie_genres.length-1">{{genre.name}},</span>
                <span v-else>{{genre.name}}</span>
              </span>
            </div>
            <div class="mb-1">
              <span>감독 :</span>
              <span v-for="(director, index) in movie.movie_directors" :key="director.id">
                <span v-if="index !== movie.movie_directors.length-1">{{director.name}},</span>
                <span v-else>{{director.name}}</span>
              </span>
            </div>
            <div class="mb-1">누적 관람객 : {{Number(movie.audience).toLocaleString()}}명</div>
            <div class="mb-1">
              <div>줄거리</div>
              <p>{{movie.summary}}</p>
            </div>
          </div>

          <hr style="background-color:white" />
          <div>
            <p class="text-center">예고편</p>
            <div v-if="movie.video_url" class="embed-responsive embed-responsive-4by3">
              <iframe class="embed-responsive-item" :src="movie.video_url" frameborder="0"></iframe>
            </div>
            <div v-else>😱</div>
          </div>
          <hr style="background-color:white" />
          <div>
            <p class="text-center">OST</p>
            <span v-if="movie.ost_url" class="embed-responsive embed-responsive-4by3">
              <iframe class="embed-responsive-item" :src="movie.ost_url" frameborder="0"></iframe>
            </span>
            <span v-else>😱</span>
          </div>

          <div>
            <hr style="background-color:white" />
            <p class="text-center">REVIEW CREATE</p>

            <div v-if="isAuthenticated">
              <div class="row">
                <div class="col-2">
                  <label for="comment">comment</label>
                </div>
                <div class="input-group col">
                  <input id="comment" class="form-control" type="text" v-model="review.comment" />
                </div>
                <div class="col-1"></div>
              </div>

              <div class="row my-3">
                <div class="col-2">
                  <label for="score" class="p-2">score</label>
                </div>
                <div class="input-group col">
                  <input
                    id="score"
                    class="form-control"
                    type="range"
                    min="0"
                    max="10"
                    step="1"
                    v-model="review.score"
                  />
                  <font v-model="review.score" class="mx-1">{{review.score}}</font>
                </div>
                <div class="col-3">
                  <button class="btn btn-primary my-3" @click="createreview">리뷰생성</button>
                </div>
                <div class="col-1"></div>
              </div>
            </div>
            <div v-else>로그인 후 리뷰를 생성할 수 있습니다.</div>
            <div>
              <hr style="background-color:white" />
              <p class="text-center">REVIEW</p>

              <div v-for="review in reviews" :key="review.id" class="my-3">
                <table class="table text-light">
                  <thead class="thead-light">
                    <tr>
                      <th scope="col">닉네임</th>
                      <th scope="col">평점</th>
                      <th scope="col">리뷰</th>
                      <th scope="col">생성시간</th>
                      <th scope="col">수정</th>
                      <th scope="col">삭제</th>
                    </tr>
                  </thead>
                  <tbody>
                    <tr>
                      <td>{{review.review_user['username']}}</td>
                      <td>{{review.score}}</td>
                      <td>{{review.comment}}</td>
                      <td>{{review.create_at}}</td>
                      <td
                        v-if="review.review_user.username === userName"
                        class="mx-2"
                        @click="updatereview(review)"
                      >🛠</td>
                      <td v-else>🚫</td>
                      <td
                        v-if="review.review_user.username === userName"
                        class="mx-2"
                        @click="deletereview(review)"
                      >🗑️</td>
                      <td v-else>🚫</td>
                    </tr>
                  </tbody>
                </table>
              </div>
            </div>
          </div>
        </div>
      </div>
      <div class="modal-footer">
        <button type="button" class="btn btn-primary" data-dismiss="modal">Close</button>
      </div>
    </div>
  </div>
</template>

<script>
import jwtDecode from "jwt-decode";
import axios from "axios";
export default {
  name: "movielistitemmodal",
  props: {
    movie: Object,
    genres: Array
  },
  data() {
    return {
      review: {
        comment: "",
        score: "5"
      },
      isAuthenticated: this.$session.has("jwt"),
      reviews: [],
      userName: ""
    };
  },
  methods: {
    getUserName() {
      this.$session.start();
      const token = this.$session.get("jwt");
      if (typeof token === "undefined") {
        this.userName = "";
      } else {
        const decodedToken = jwtDecode(token);
        this.userName = decodedToken.username;
        console.log(this.userName);
      }
    },
    createreview() {
      const token = this.$session.get("jwt");
      const header = {
        headers: {
          Authorization: `JWT ${token}`
        }
      };
      axios
        .post(
          `http://127.0.0.1:8000/api/v1/movies/${this.movie.id}/reviews/`,
          this.review,
          header
        )
        .then(response => {
          const data = response.data;
          this.reviews.push(data);

          this.comment = "";
          this.score = "";
          // 왜 초기화 안됨ㅠ 수정 필요
        })
        .catch(error => {
          console.log(error);
        });
    },
    deletereview: function(review) {
      const token = this.$session.get("jwt");
      const header = {
        headers: {
          Authorization: `JWT ${token}`
        }
      };
      axios
        .delete(
          `http://127.0.0.1:8000/api/v1/movies/reviews/${review.id}/`,
          header
        )
        .then(() => {
          const targetreview = this.reviews.find(function(el) {
            return el === review;
          });
          const idx = this.reviews.indexOf(targetreview);
          if (idx > -1) {
            this.reviews.splice(idx, 1);
          }
        })
        .catch(error => {
          console.log(error);
        });
    },
    updatereview: function(review) {
      const token = this.$session.get("jwt");
      const header = {
        headers: {
          Authorization: `JWT ${token}`
        }
      };
      axios
        .put(
          `http://127.0.0.1:8000/api/v1/movies/reviews/${review.id}/`,
          this.review,
          header
        )
        .then(response => {
          const targetreview = this.reviews.find(function(el) {
            return el === review;
          });
          const idx2 = this.reviews.indexOf(targetreview);
          this.reviews[idx2].comment = response.data.comment;
          this.reviews[idx2].score = response.data.score;
        })
        .catch(error => {
          console.log(error);
        });
    }
  },
  mounted() {
    this.getUserName();
    axios
      .get(`http://127.0.0.1:8000/api/v1/movies/${this.movie.id}/reviewall/`)
      .then(res => {
        this.reviews = res.data;
      })
      .catch(err => console.log(err));
  }
};
</script>

<style>
#img {
  transition: transform 0.2s; /* Animation */
}
#img:hover {
  opacity: 50%;
  -webkit-transform: scale(1.2);
  transform: scale(1.2);
}
</style>