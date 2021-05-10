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
import Header from "../components/Header.vue";
import Table from "../components/EmployeesTable.vue";
import PersonalAccount from "./PersonalAccount.vue";
import { config } from "../config/config";
import { data } from "../config/dummyData";
export default {
  components: { Header, Table, PersonalAccount },
  data() {
    return {
      rawData: data,
      title: config.titles,
      isModalOpen: false,
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

