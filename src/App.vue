<template>
  <div class="container">
    <form class="card" @submit.prevent="createPersen">
      <h2>Работа с базой данных</h2>
      <div class="form-control">
        <label for="">Введите текст</label>
        <input type="text" v-model.trim="name" />
      </div>
      <button class="btn primary" :disabled="name.length === 0">
        Создать человека
      </button>
    </form>
    <app-new-people @load="loadPeople" :people="people"></app-new-people>
  </div>
</template>

<script>
import AppNewPeople from "./components/AppNewPeople.vue";
import axios from "axios";
export default {
  data() {
    return {
      name: "",
      people: [],
    };
  },
  mounted() {
    this.loadPeople();
  },
  methods: {
    async createPersen() {
      const response = await fetch(
        "https://vue-with-http-3c549-default-rtdb.firebaseio.com/people.json",
        {
          method: "POST",
          headers: {
            "Content-Type": "aplication/json",
          },
          body: JSON.stringify({
            firstName: this.name,
          }),
        }
      );
      const firstBaseData = await response.json();
      console.log(firstBaseData);
      this.name = "";
      this.people.push({
        firstName: this.name,
        id: firstBaseData.name,
      });
    },
    async loadPeople() {
      const { data } = await axios.get(
        "https://vue-with-http-3c549-default-rtdb.firebaseio.com/people.json"
      );
      const result = Object.keys(data).map((key) => {
        return {
          id: key,
          firstName: data[key].firstName,
        };
      });
      this.people = result;
    },
  },
  components: { AppNewPeople },
};
</script>

<style>
</style>

