<template>
  <div id="app">
    <div id="nav" class="d-flex justify-content-between">
      <div class="row">
        <!-- <span id="mango">
          Flying
        </span>-->
        <div class="col">
          <img src="mango.png" alt style="width: 70px" />
        </div>
        <!-- <span id="mango">
          Mango
        </span>-->
        <div id="mango" class="pt-1">Flying Mango</div>
      </div>
      <div class="d-flex align-items-center">
        <router-link to="/">Genre</router-link>|
        <router-link to="/Director">Director</router-link>|
        <span v-if="isAuthenticated">
          <router-link to="/recommend">Recommend Movie</router-link>|
          <router-link to="/mypage">{{username}}님 페이지</router-link>|
          <a href="#" @click.prevent="logout">Logout</a>
        </span>
        <span v-else>
          <router-link to="/login">Login</router-link>|
          <router-link to="/signup">Signup</router-link>
        </span>
      </div>
    </div>

    <div class="container my-3">
      <router-view />
    </div>
  </div>
</template>


<script>
import jwtDecode from "jwt-decode";

export default {
  name: "App",
  components: {},
  data: function() {
    return {
      isAuthenticated: this.$session.has("jwt"),
      username: ""
    };
  },
  methods: {
    logout: function() {
      this.$session.destroy();
      this.$router.push("/login");
    },
    getusername: function() {
      this.$session.start();
      const token = this.$session.get("jwt");
      const decodedToken = jwtDecode(token);
      this.username = decodedToken.username;
    }
  },
  updated: function() {
    this.isAuthenticated = this.$session.has("jwt");
    this.getusername();
  },
  mounted() {
    this.getusername();
  }
};
</script>


<style>
@font-face {
  font-family: "HangeulNuri-Bold";
  src: url("https://cdn.jsdelivr.net/gh/projectnoonnu/noonfonts_three@1.0/HangeulNuri-Bold.woff")
    format("woff");
  font-weight: normal;
  font-style: normal;
}
@font-face {
  font-family: "BBTreeGB";
  src: url("https://cdn.jsdelivr.net/gh/projectnoonnu/noonfonts_nine_@1.1/BBTreeGB.woff")
    format("woff");
  font-weight: normal;
  font-style: normal;
}
@font-face {
  font-family: "양진체";
  src: url("https://cdn.jsdelivr.net/gh/supernovice-lab/font@0.9/yangjin.woff")
    format("woff");
  font-weight: normal;
  font-style: normal;
}
#app {
  /* font-family: 'Avenir', Helvetica, Arial, sans-serif; */
  font-family: "BBTreeGB";
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #e08119;
}

#nav {
  padding: 30px;
  background-color: #2a293e;
  font-size: 1.25rem;
}

#nav a {
  font-weight: bold;
  color: #9f9c99;
}

#nav a.router-link-exact-active {
  color: #f3e0be;
}

#mango {
  font-family: "HangeulNuri-Bold";
  font-size: 2.25rem;
}
</style>