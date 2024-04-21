<template>
  <div class="card" v-if="people.length !== 0">
    <div v-for="persens in people" :key="persens.id">
      <h3>{{ persens.firstName }}</h3>
    </div>
  </div>
  <div class="card center" v-else>
    <h4>Людей пока нет.</h4>
    <br />
    <button class="btn" @click="$emit('load')">Загрузить список людей</button>
  </div>
</template>

<script>
export default {
  emits: ["load"],
  props: ["people"],
};
</script>

<style>
</style>