<template>
  <div class="title container">
    <div class="title text">
      <h2>{{ namespace.employeesList.ru }}</h2>
    </div>
  </div>
</template>

<script>
import { config } from "../config/config";
export default {
  data() {
    return {
      namespace: config.titles,
    };
  },
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
