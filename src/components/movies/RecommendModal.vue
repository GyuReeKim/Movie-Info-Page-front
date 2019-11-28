<template>
  <div
    class="modal fade"
    tabindex="-1"
    role="dialog"
    v-bind:id="`gg-${choice.id}`"
    data-backdrop="static"
    data-keypress="false"
  >
    <div class="modal-dialog modal-lg" role="document">
      <div class="modal-content bg-dark text-white">
        <div class="modal-header">
          <h5 class="modal-title">🎬 {{choice.title}} ({{choice.title_en}})</h5>
          <button type="button" class="close text-white" data-dismiss="modal" aria-label="Close">
            <span aria-hidden="true">&times;</span>
          </button>
        </div>
        <div class="modal-body">
          <div id="img">
            <img
              class="movie--poster my-3"
              v-bind:src="choice.poster_url"
              v-bind:alt="choice.title"
              style="width:50%"
            />
          </div>
          <hr style="background-color:white" />
          <p class="text-center">DETAIL</p>
          <div>평점 : {{choice.score}}</div>
          <div>누적 관람객 : {{choice.audience}}명</div>
          <div>
            감독 :
            <span v-for="director in choice.movie_directors" :key="director">{{director.name}}</span>
          </div>
          <div class="my-1">
            장르 :
            <span v-for="genre in choice.movie_genres" :key="genre">{{genre.name}}</span>
          </div>
          <div class="text-left">{{choice.summary}}</div>

          <hr style="background-color:white" />
          <p class="text-center">예고편</p>
          <div v-if="choice.video_url" class="embed-responsive embed-responsive-4by3">
            <iframe class="embed-responsive-item" :src="choice.video_url" frameborder="0"></iframe>
          </div>
          <div v-else>😱</div>

          <hr style="background-color:white" />
          <p class="text-center">OST</p>
          <div v-if="choice.ost_url" class="embed-responsive embed-responsive-4by3">
            <iframe class="embed-responsive-item" :src="choice.ost_url" frameborder="0"></iframe>
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
  name: "RecommendModal",

  props: {
    choice: Object
  },
  data() {
    return {};
  },
  methods: {}
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