<template>
  <Header />
  <Table />
</template>

<script>
import Header from "../components/Header.vue";
import Table from "../components/EmployeesTable.vue";
export default {
  components: { Header, Table },
  data() {
    return {};
  },
};
</script>

