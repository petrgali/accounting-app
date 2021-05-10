<template>
  <Header :title="title.employeesList.ru" />
  <Table :rawData="rawData" @open-person="openModal" />
  <PersonalAccount
    v-if="modalCheck"
    @close-modal="personData = null"
    :data="personData"
  />
</template>

<script>
import { data } from "../config/dummyData";
import { config } from "../config/config";
import Header from "../components/Header";
import Table from "../components/EmployeesTable";
import PersonalAccount from "./PersonalAccount";
export default {
  components: { Header, Table, PersonalAccount },
  data() {
    return {
      rawData: data,
      title: config.titles,
      personData: null,
    };
  },
  methods: {
    openModal(id) {
      this.personData = this.rawData.filter((person) => person.id === id)[0];
    },
  },
  computed: {
    modalCheck() {
      return this.personData ? true : false;
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
  z-index: 1;
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
  margin: auto auto;
}
.responsive-table .table-header {
  background-color: #95a5a6;
  font-size: 14px;
  text-transform: uppercase;
  letter-spacing: 0.03em;
  cursor: pointer;
}
.responsive-table .table-row {
  align-items: center;
  cursor: pointer;
  border-top: 2px solid lightgrey;
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
