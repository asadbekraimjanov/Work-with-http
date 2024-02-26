<template>
  <div v-if="people.length !== 0">
    <div class="card mt-1" v-for="(person, index) in people" :key="person.id">
      <h3>{{ person.name }}</h3>
      <button class="btn danger" @click="$emit('remove', person.id, index)">Delete</button>
    </div>
  </div>
  <div class="card mt-1" v-else>
    <h4>List is empty.</h4>
    <button @click="$emit('load')" class="btn button-reload">Reload list</button>
  </div>
</template>

<script>
export default {
  emits: ['load', 'remove'],
  props: ['people']
}
</script>

<style scoped>

.danger{
  margin: 10px;
  width: 140px;
  height: 36px;
  background-color: #ffffff;
  border-radius: 20px;
  background-color: #ec5534;
  font-weight: 600;
}
.card h3, .card h4{
  color: #2c3e50;
  margin: 10px;
}
.button-reload{
  margin: 10px;
  width: 140px;
  height: 36px;
  background-color: #ffffff;
  border-radius: 20px;
  border: 1px solid mediumseagreen;
  color: mediumseagreen;
  font-weight: 600;
}
.button-reload:active{
  background-color: mediumseagreen;
  border: 1px solid #ffffff;
  color: #ffffff;
}
</style>
