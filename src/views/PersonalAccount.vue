<template>
  <div class="modal">
    <div class="modal-content">
      <Header :title="title.goodsList.ru" />
      <div class="person-info">
        <div class="titles">
          <div v-if="addNew">
            <div class="item">
              {{ title.secondName.ru }}:<input
                type="text"
                v-model="newPerson.secondName"
              />
            </div>
            <div class="item">
              {{ title.firstName.ru }}:<input
                type="text"
                v-model="newPerson.name"
              />
            </div>
            <div class="item">
              {{ title.patronName.ru }}:<input
                type="text"
                v-model="newPerson.patronName"
              />
            </div>
          </div>
          <div v-if="!addNew">
            <div class="item">
              {{ title.secondName.ru }}: {{ person.secondName }}
            </div>
            <div class="item">
              {{ title.firstName.ru }}: {{ person.name }}
              </div>
            <div class="item">
              {{ title.patronName.ru }}: {{ person.patronName }}
            </div>
          </div>
        </div>
      </div>
      <PersonalTable :personData="person" @close-modal="$emit(`closeModal`)" />
    </div>
  </div>
</template>

<script>
import PersonalTable from "../components/PersonalTable";
import Header from "../components/Header";
import { config } from "../config/config";
export default {
  props: ["data"],
  components: { PersonalTable, Header },
  data() {
    return {
      addNew: false,
      title: config.titles,
      person: this.data,
      newPerson: {
        name: "",
        secondName: "",
        patronName: "",
      },
    };
  },
};
</script>

<style>
.person-info {
  display: flex;
  justify-content: center;
}
.titles {
  display: flex;
  flex-direction: column;
  width: 75%;
}
.titles .item {
  display: flex;
  justify-content: space-between;
  align-items: center;
  width: 50%;
  text-align: left;
  margin: 5px 2px;
}
.modal {
  position: fixed;
  z-index: 1;
  left: 0;
  top: 0;
  width: 100%;
  height: 100%;
  overflow: auto;
  background-color: rgba(0, 0, 0, 0.7);
}
.modal-content {
  background-color: #fefefe;
  margin: 15% auto;
  padding: 20px;
  border: 1px solid #888;
  width: 80%;
  height: 50%;
  overflow: auto;
}
</style>