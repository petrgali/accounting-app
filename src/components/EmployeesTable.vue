<template>
  <div class="container">
    <div class="filter">
      <select name="fields" class="select" v-model="filterSelect">
        <option :value="titles.fullName.ru">
          {{ titles.fullName.ru }}
        </option>
        <option :value="titles.countValue.ru">
          {{ titles.countValue.ru }}
        </option>
        <option :value="titles.amount.ru">
          {{ titles.amount.ru }}
        </option>
      </select>
      <div
        v-if="
          filterSelect === titles.countValue.ru ||
          filterSelect === titles.amount.ru
        "
      >
        <select class="select" v-model="filterOperator">
          <option value="lower">{{ buttonsTitle.lower }}</option>
          <option value="equal">{{ buttonsTitle.equal }}</option>
          <option value="bigger">{{ buttonsTitle.bigger }}</option>
        </select>
      </div>
      <input
        type="search"
        class="filter-input"
        v-model="filterText"
        @input="filterTable"
      />
      <Button :meta="removeHighlightButton" @click="resetHighlight" />
    </div>

    <ul class="responsive-table">
      <li class="table-header">
        <div class="col col-1" @click="sortFullname">
          {{ titles.fullName.ru }}
        </div>
        <div class="col col-2" @click="sortCountValue">
          {{ titles.countValue.ru }}
        </div>
        <div class="col col-3" @click="sortAmount">
          {{ titles.amount.ru }}
        </div>
      </li>
      <li
        class="table-row"
        v-for="person in data"
        :id="person.id"
        :class="highlightRow(person.id)"
        :key="person.id"
        @click="selectRow"
        @contextmenu.prevent="ctxMenu"
        @dblclick="$emit(`openPerson`, person.id)"
      >
        <TableRow :person="person" />
      </li>
    </ul>
    <Button :meta="addRecordButton" />
  </div>
</template>

<script>
import TableRow from "./EmpoyeesTableRow";
import { config } from "../config/config";
import Button from "../components/Button";
export default {
  components: { TableRow, Button },
  props: ["rawData"],
  data() {
    return {
      removeHighlightButton: {
        title: config.buttons.removeHighlight,
        class: "filter-button",
      },
      addRecordButton: {
        title: config.buttons.addRecord,
      },
      titles: config.titles,
      buttonsTitle: config.buttons,
      data: this.rawData,
      highlighted: null,
      sort: {
        name: false,
        value: false,
        amount: false,
      },
      filterText: "",
      filterSelect: "",
      filterOperator: "",
    };
  },
  methods: {
    getRowId(e) {
      return e.target.id === "" ? +e.target.parentNode.id : +e.target.id;
    },
    selectRow(e) {
      this.highlighted = this.getRowId(e);
    },
    highlightRow(id) {
      return id === this.highlighted ? "selected" : "";
    },
    resetHighlight() {
      this.highlighted = null;
    },
    ctxMenu(e) {
      if (this.getRowId(e) === this.highlighted) confirm(this.highlighted);
    },
    sortFullname() {
      this.data.sort((a, b) => {
        return this.sort.name
          ? b.secondName.toUpperCase() > a.secondName.toUpperCase()
          : a.secondName.toUpperCase() > b.secondName.toUpperCase();
      });
      this.sort.name = !this.sort.name;
    },
    sortCountValue() {
      this.data.sort((a, b) => {
        return this.sort.value
          ? b.goods.length - a.goods.length
          : a.goods.length - b.goods.length;
      });
      this.sort.value = !this.sort.value;
    },
    getTotalAmount(arr) {
      return arr.goods.reduce((total, item) => {
        return (total += item.itemCost);
      }, 0);
    },
    sortAmount() {
      this.data.sort((a, b) => {
        return this.sort.amount
          ? this.getTotalAmount(b) - this.getTotalAmount(a)
          : this.getTotalAmount(a) - this.getTotalAmount(b);
      });
      this.sort.amount = !this.sort.amount;
    },
    filterWithOperator(a, b) {
      switch (this.filterOperator) {
        case "lower":
          return a < b;
        case "equal":
          return a === b;
        case "bigger":
          return a > b;
      }
    },
    filterTable() {
      if (this.filterText)
        switch (this.filterSelect) {
          case this.titles.fullName.ru:
            this.data = this.$props.rawData.filter((person) =>
              person.secondName.toLowerCase().includes(this.filterText)
            );
            break;
          case this.titles.countValue.ru:
            this.data = this.$props.rawData.filter((person) =>
              this.filterWithOperator(person.goods.length, +this.filterText)
            );
            break;
          case this.titles.amount.ru:
            this.data = this.$props.rawData.filter((person) =>
              this.filterWithOperator(
                this.getTotalAmount(person),
                +this.filterText
              )
            );
        }
      else {
        this.data = this.$props.rawData;
      }
    },
  },
};
</script>
