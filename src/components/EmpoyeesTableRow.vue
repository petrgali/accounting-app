<template>
  <div class="col col-1" :data-label="titles.fullName.ru">
    {{ shortName }}
  </div>
  <div class="col col-2" :data-label="titles.countValue.ru">
    {{ this.$props.person.goods.length }}
  </div>
  <div class="col col-3" :data-label="titles.amount.ru">
    {{ totalItemsCost }}
  </div>
</template>

<script>
import { config } from "../config/config.js";
export default {
  props: ["person", "data"],
  data() {
    return {
      titles: config.titles,
    };
  },
  computed: {
    shortName() {
      return this.$props.person.secondName
        .concat(" ")
        .concat(this.$props.person.name.slice(0, 1))
        .concat(".")
        .concat(this.$props.person.patronName.slice(0, 1))
        .concat(".");
    },
    totalItemsCost() {
      return this.$props.person.goods.reduce((total, item) => {
        return (total += item.itemCost);
      }, 0);
    },
  },
};
</script>

<style>
</style>