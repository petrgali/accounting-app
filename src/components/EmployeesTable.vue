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
          <option value="lower">меньше</option>
          <option value="equal">равно</option>
          <option value="bigger">больше</option>
        </select>
      </div>
      <input
        type="search"
        class="filter-input"
        v-model="filterText"
        @input="filterTable"
      />
      <button class="filter-button" @click="resetHighlight">
        снять выделение
      </button>
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
        @dblclick="doubleClickHandler"
      >
        <TableRow :person="person" />
      </li>
    </ul>
  </div>
</template>

<script>
import { rawData } from "../config/dummyData";
import TableRow from "./EmpoyeesTableRow";
import { config } from "../config/config";
export default {
  components: { TableRow },
  data() {
    return {
      titles: config.titles,
      data: rawData,
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
      if (this.getRowId(e) === this.highlighted) alert(this.highlighted);
    },
    doubleClickHandler(e) {
      alert(e.target);
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
            this.data = rawData.filter((person) =>
              person.secondName.toLowerCase().includes(this.filterText)
            );
            break;
          case this.titles.countValue.ru:
            this.data = rawData.filter((person) =>
              this.filterWithOperator(person.goods.length, +this.filterText)
            );
            break;
          case this.titles.amount.ru:
            this.data = rawData.filter((person) =>
              this.filterWithOperator(
                this.getTotalAmount(person),
                +this.filterText
              )
            );
        }
      else {
        this.data = rawData;
      }
    },
  },
};
</script>

<style>
.container {
  max-width: 1000px;
  margin-left: auto;
  margin-right: auto;
  padding-left: 10px;
  padding-right: 10px;
}
h2 {
  font-size: 26px;
  margin: 20px 0;
  text-align: center;
}
h2 small {
  font-size: 0.5em;
}
.filter {
  display: flex;
  width: 80%;
  margin: auto;
  justify-content: left;
}
.filter .select {
  width: 100px;
  min-width: 15%;
  margin: auto 5px;
}
.filter-input {
  width: 150px;
  padding: 5px 5px;
}
.filter-button {
  margin: auto 5px;
}
.table-row.selected {
  background-color: rgb(49, 197, 20);
  color: aliceblue;
}
.responsive-table li {
  border-radius: 3px;
  padding: 25px 30px;
  display: flex;
  width: 80%;
  justify-content: space-between;
  margin: auto auto 25px;
}
.responsive-table .table-header {
  background-color: #95a5a6;
  font-size: 14px;
  text-transform: uppercase;
  letter-spacing: 0.03em;
  cursor: pointer;
}
.responsive-table .table-row {
  cursor: pointer;
}
.responsive-table .col-1 {
  flex-basis: 40%;
}
.responsive-table .col-2 {
  flex-basis: 20%;
}
.responsive-table .col-3 {
  flex-basis: 40%;
}
/* .responsive-table .col-4 {
  flex-basis: 25%;
} */
@media all and (max-width: 767px) {
  .responsive-table .table-header {
    display: none;
  }
  .responsive-table li {
    display: block;
  }
  .responsive-table .col {
    flex-basis: 100%;
  }
  .responsive-table .col {
    display: flex;
    padding: 10px 0;
  }
  .responsive-table .col:before {
    color: #6c7a89;
    padding-right: 10px;
    content: attr(data-label);
    flex-basis: 50%;
    text-align: right;
  }
}
</style>