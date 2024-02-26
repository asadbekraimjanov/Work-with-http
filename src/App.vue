<template>
  <div class="container mt-3">
    <form class="card" @submit.prevent="createPerson">
      <h2>Work with http</h2>

      <div class="form-control">
        <label for="name">Name</label>
        <input type="text" id="name" v-model.trim="name">
      </div>

      <button class="btn primary" :disabled="name.length === 0">Create person</button>
    </form>

    <app-people-list @load="loadPeople" @remove="removePeople" :people="people"></app-people-list>
  </div>
</template>

<script>

import axios from "axios";
import AppPeopleList from "@/components/AppPeopleList.vue";
import {ElMessage} from "element-plus";
export default {
  data() {
    return {
      name: '',
      people: [],
      alert: null
    }
  },
  mounted(){
    this.loadPeople()
  },
  methods: {
    async createPerson() {
      const response = await fetch('https://with-http-62315-default-rtdb.firebaseio.com/people.json', {
        method: 'POST',
        headers: {
          'Content-Type': 'application/json'
        },
        body: JSON.stringify({
          name: this.name
        })
      })

      const firebaseData = await response.json()

      this.people.push({
        name: this.name,
        id: firebaseData.name
      })
      this.name = ''
    },

    async loadPeople() {
      try {
        const {data} = await axios.get('https://with-http-62315-default-rtdb.firebaseio.com/people.json', )
        this.people = Object.keys(data).map(key => {
          return {
            id: key,
            ...data[key]    //name: data[key].name
          }
        })
      } catch (e) {
        this.alert = 'Bazadan ma\'lumot olinmadi!'
      }
      if (this.alert){
        ElMessage({
          message: this.alert,
          type: "error"
        })
      }
    },

    async removePeople(id, index){
      try {
        await axios.delete(`https://with-http-62315-default-rtdb.firebaseio.com/people/${id}.json`)
        ElMessage({
          message: `${this.people[index].name} ismli foydalanuvchi muvaffaqiyatli o\'chirildi.`,
          type: "success"
        })
        this.people = this.people.filter(person => person.id !== id)
      } catch (e) {
        ElMessage({
          message: e.message,
          type: "error"
        })
      }
    }
  },
  components: {AppPeopleList}
}
</script>

<style>
body{
  background-color: #2c3e50;
}
.card h2{
  margin: 10px;
  font-weight: 600;
  color: #2c3e50;
}
.form-control{
  padding: 15px;
}
.form-control label{
  display: block;
  font-weight: 600;
  color: #2c3e50;
}
.form-control input{
  border: 2px solid #d9d9d9;
  border-radius: 3px;
  width: 100%;
  height: 35px;
}
.btn.primary{
  margin: 10px;
  width: 140px;
  background-color: mediumseagreen;
  border-radius: 20px;
  color: white;
  font-weight: 600;
}
</style>
