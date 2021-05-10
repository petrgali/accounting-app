<template>
  <div class="container">
    <ul class="responsive-table">
      <li class="table-header">
        <div class="col col-1">{{ titles.itemNumber.ru }}</div>
        <div class="col col-2">{{ titles.itemName.ru }}</div>
        <div class="col col-3">{{ titles.itemCost.ru }}</div>
      </li>
      <li class="table-row" v-for="(good, idx) in person.goods" :key="good">
        <div class="col col-1 left">{{ idx + 1 }}</div>
        <div class="col col-2 left">{{ good.item }}</div>
        <div class="col col-3 right">{{ good.itemCost }}</div>
      </li>
      <li class="table-row">
        <div class="col col-4 right">{{ titles.totalCost.ru }}</div>
        <div class="col col-3 right">{{ totalCost }}</div>
      </li>
    </ul>
    <button style ="margin-bottom: 20px" @click="$emit(`closeModal`)">close</button>
  </div>
</template>

<script>
import { config } from "../config/config";
export default {
  props: ["personData"],
  data() {
    return {
      person: this.personData,
      titles: config.titles,
    };
  },
  computed: {
    totalCost() {
      return this.person.goods.reduce((total, item) => {
        return (total += item.itemCost);
      }, 0);
    },
  },
};
</script>

<style scoped>
.responsive-table .col-1 {
  flex-basis: 20%;
}
.responsive-table .col-2 {
  flex-basis: 40%;
}
.responsive-table .col-3 {
  flex-basis: 40%;
}
.responsive-table .col-4 {
  flex-basis: 60%;
}
.left {
  text-align: left;
}
.right {
  text-align: right;
}
</style>>