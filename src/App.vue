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
    <app-new-people 
     @load="loadPeople"
     :people="people"
     @remove="removePeople"
     ></app-new-people>
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
      this.people.push({
        firstName: this.name,
        id: firstBaseData.name,
      });
      this.name = "";
    },
    async loadPeople() {
      try {
        const { data } = await axios.get(
        "https://vue-with-http-3c549-default-rtdb.firebaseio.com/people.json"
      );
      console.log(data)
      const res = Object.keys(data).map(key => {
        return {
          id: key,
          ...data[key]
        };
      });
      this.people = res;
      } catch (e) {
        console.log(e.message)
      }
    },
   async removePeople (id) {
      await axios.delete(`https://vue-with-http-3c549-default-rtdb.firebaseio.com/people/${id}.json`)
      this.people = this.people.filter(people => people.id !== id)
    }
  },
  components: { AppNewPeople },
};
</script>

<style>
</style>

