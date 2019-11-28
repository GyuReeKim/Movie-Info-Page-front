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
          <img
            class="movie--poster my-3 mx-auto"
            v-bind:src="movie.poster_url"
            v-bind:alt="movie.title"
            style="width:50%"
          />
          <hr style="background-color:white" />
          <p class="text-center">DETAIL</p>
          <div>평점 : {{movie.score}}</div>
          <p>누적 관람객 : {{movie.audience}}명</p>
          <p>{{movie.summary}}</p>

          <hr style="background-color:white" />
          <p class="text-center">예고편</p>
          <div v-if="movie.video_url" class="embed-responsive embed-responsive-4by3">
            <iframe class="embed-responsive-item" :src="movie.video_url" frameborder="0"></iframe>
          </div>
          <div v-else>😱</div>

          <hr style="background-color:white" />
          <p class="text-center">OST</p>
          <div v-if="movie.ost_url" class="embed-responsive embed-responsive-4by3">
            <iframe class="embed-responsive-item" :src="movie.ost_url" frameborder="0"></iframe>
          </div>
          <div v-else>😱</div>
        </div>
        <div class="modal-footer">
          <button type="button" class="btn btn-primary" data-dismiss="modal">Close</button>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "Directorlistitemmodal",

  props: {
    movie: Object,
    genres: Array
  },
  data() {
    return {
      review: {
        comment: "",
        socre: ""
      },
      isAuthenticated: this.$session.has("jwt")
    };
  },
  methods: {
    createreview() {}
  }
};
</script>

<style>
</style>