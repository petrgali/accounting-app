<template>
  <div class="title container">
    <div class="title text">
      <h2>{{ title }}</h2>
    </div>
  </div>
</template>

<script>
export default {
  props: ["title"],
};
</script>

<style>
.title {
  display: flex;
}
.container {
  justify-content: center;
  height: 70px;
  width: 100%;
  margin-top: 50px;
}
.text {
  width: 80%;
}
</style>
