<template>
  <div class="container">
    <ul class="responsive-table">
      <li class="table-header">
        <div class="col col-1">{{ titles.itemNumber.ru }}</div>
        <div class="col col-2">{{ titles.itemName.ru }}</div>
        <div class="col col-3">{{ titles.itemCost.ru }}</div>
        <div class="col col-4"></div>
      </li>
      <li class="table-row" v-for="(good, idx) in person.goods" :key="good">
        <div class="table-wrap" v-if="!editMode">
          <div class="col col-1 left">{{ idx + 1 }}</div>
          <div class="col col-2 left">{{ good.item }}</div>
          <div class="col col-3 right">{{ good.itemCost }}</div>
          <div class="col col-4"></div>
          <img
            class="custom-button"
            src="../assets/trash.png"
            @click="removeItem(idx)"
            alt="remove"
          />
        </div>
        <div class="table-wrap" v-if="editMode">
          <div class="col col-1 left">{{ idx + 1 }}</div>
          <div class="col col-2 left"><input type="text" v-model="editedItem.name"/></div>
          <div class="col col-3 right"><input type="text" v-model="editedItem.cost"/></div>
          <div class="col col-4"></div>
        </div>
      </li>
      <li class="table-row">
        <div class="col col-5 right">{{ titles.totalCost.ru }}</div>
        <div class="col col-3 right">{{ totalCost }}</div>
        <div class="col col-4"></div>
         <img v-if="!editMode"
            class="custom-button"
            src="../assets/edit.png"
            @click="editItem(idx)"
            alt="edit"
          />
      </li>
    </ul>
    <Button v-if="!editMode" :meta="closeButton" @click="$emit(`closeModal`)" />
    <div v-if="editMode">
      <Button :meta="saveButton" @click="editItem" />
      <Button :meta="cancelButton" @click="editItem" />
    </div>
  </div>
</template>

<script>
import { config } from "../config/config";
import Button from "./Button.vue";
export default {
  props: ["personData"],
  components: { Button },
  data() {
    return {
      closeButton: {
        class: "bottom",
        title: config.buttons.close,
      },
      cancelButton: {
        class: "bottom",
        title: config.buttons.cancel,
      },
      saveButton: {
        class: "bottom",
        title: config.buttons.saveRecord,
      },
      editMode: false,
      person: this.personData,
      titles: config.titles,
      editedItem: {
        name: "",
        cost: "",
      },
    };
  },
  methods: {
    removeItem(id) {
      this.person.goods = this.person.goods.filter((_, idx) => idx !== id);
    },
    editItem() {
      this.editMode = !this.editMode;
    },
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
.bottom {
  margin: 20px 5px;
  width: 8em;
}
.table-wrap {
  display: flex;
  align-items: center;
  width: 100%;
}
.custom-button {
  width: 40px;
  height: 40px;
  margin: auto 5px;
}
.custom-button:hover {
  box-shadow: rgba(0, 0, 0, 0.3) 0px 19px 38px,
    rgba(0, 0, 0, 0.22) 0px 15px 12px;
  border-radius: 800px;
}
.responsive-table .col-1 {
  flex-basis: 15%;
}
.responsive-table .col-2 {
  flex-basis: 40%;
}
.responsive-table .col-3 {
  flex-basis: 30%;
}
.responsive-table .col-4 {
  flex-basis: 20%;
}
.responsive-table .col-5 {
  flex-basis: 55%;
}
.left {
  text-align: left;
}
.right {
  text-align: right;
}
</style>>