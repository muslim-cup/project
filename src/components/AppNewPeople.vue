<template>
  <div class="card inline" v-if="people.length !== 0">
    <div v-for="persens in people" :key="persens.id">
      <h3>{{ persens.firstName }}</h3>
      <button class="btn danger" @click="$emit('remove', persens.id)">Удалить</button>
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
  emits: ["load","remove"],
  props: ["people"],
};
</script>

<style scoped>
 .inline {
  display: flex;
  justify-content: space-between;
  align-items: center;
 }
</style>