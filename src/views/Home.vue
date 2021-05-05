<template>
  <Header />
  <div class="home">
    <h1>{{ msg }}</h1>
  </div>
</template>

<script>
import Header from "../components/Header.vue";
export default {
  components: { Header },
  data() {
    return {
      msg: "Welcome to Your Vue.js App",
    };
  },
};
</script>
