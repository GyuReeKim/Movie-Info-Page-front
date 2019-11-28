<template>
  <div class="container my-5">
    <div class="py-2">
      <div class="section__item">
        <div style="color: #E08119;" href="#0" class="sm-link sm-link_padding-all sm-link5">
          <h3>👨‍💻 {{username}}님의 페이지 👩‍💻</h3>
        </div>
      </div>
    </div>

    <div v-if="reviews.length">
      <div class="tiles row">
        <div
          class="tile col-12 col-md-6 col-lg-3 my-5 mx-auto"
          v-for="review in reviews"
          :key="review.id"
        >
          <div class="card">
            <img :src="review.movie.poster_url" />
          </div>
          <div class="details">
            <div class="m-2">
              <span style="font-size: 1.5rem" class="title">{{review.movie.title}}</span>
            </div>
            <div class="m-2 pt-1">
              <span style="font-size: 1.05rem" class="info">{{review.comment}}</span>
            </div>
          </div>
        </div>
      </div>
    </div>
    <div v-else>
      <div class="jumbotron jumbotron-fluid my-5 bg-danger text-light" id="img">
        <div class="container">
          <h1 class="display-4">😭리뷰가 없습니다.😥</h1>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import jwtDecode from "jwt-decode";
import axios from "axios";

export default {
  name: "MypageForm",
  data() {
    return {
      reviews: [],
      username: ""
    };
  },
  methods: {
    // 함수
    getReviews() {
      this.$session.start();
      const token = this.$session.get("jwt");
      const decodedToken = jwtDecode(token);
      // console.log(decodedToken)
      const user_id = decodedToken.user_id;
      // console.log(user_id);

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
          // console.log(this.reviews);
        })
        .catch(error => {
          console.log(error);
        });
    }
  },
  mounted: function() {
    this.getReviews();
  }
};
</script>

<style>
@import url(//codepen.io/chrisdothtml/pen/ojLzJK.css);
/* .tiles {
  width: 1040px;
  font-size: 0;
  text-align: center;
  position: absolute;
  top: 50%;
  left: 50%;
  -webkit-transform: translate(-50%, -50%);
          transform: translate(-50%, -50%);
} */
.tiles .tile {
  display: inline-block;
  margin: 10px;
  text-align: left;
  opacity: 0.99;
  overflow: hidden;
  position: relative;
  border-radius: 3px;
  box-shadow: 0 0 20px 0 rgba(0, 0, 0, 0.05);
}
.tiles .tile:before {
  content: "";
  background: linear-gradient(
    to bottom,
    rgba(0, 0, 0, 0) 0%,
    rgba(0, 0, 0, 0.7) 100%
  );
  width: 100%;
  height: 50%;
  opacity: 0;
  position: absolute;
  top: 100%;
  left: 0;
  z-index: 2;
  transition-property: top, opacity;
  transition-duration: 0.3s;
}
.tiles .tile img {
  display: block;
  max-width: 100%;
  backface-visibility: hidden;
  -webkit-backface-visibility: hidden;
}
.tiles .tile .details {
  font-size: 16px;
  padding: 20px;
  color: #fff;
  position: absolute;
  bottom: 0;
  left: 0;
  z-index: 3;
}
.tiles .tile .details span {
  display: block;
  opacity: 0;
  position: relative;
  top: 100px;
  transition-property: top, opacity;
  transition-duration: 0.3s;
  transition-delay: 0s;
}
.tiles .tile .details .title {
  line-height: 1;
  font-weight: 600;
  font-size: 18px;
}
.tiles .tile .details .info {
  line-height: 1.2;
  margin-top: 5px;
  font-size: 12px;
}
.tiles .tile:focus:before,
.tiles .tile:focus span,
.tiles .tile:hover:before,
.tiles .tile:hover span {
  opacity: 1;
}
.tiles .tile:focus:before,
.tiles .tile:hover:before {
  top: 50%;
}
.tiles .tile:focus span,
.tiles .tile:hover span {
  top: 0;
}
.tiles .tile:focus .title,
.tiles .tile:hover .title {
  transition-delay: 0.15s;
}
.tiles .tile:focus .info,
.tiles .tile:hover .info {
  transition-delay: 0.25s;
}
</style>