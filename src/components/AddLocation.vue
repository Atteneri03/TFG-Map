<template>
  <form @submit.prevent>
    <h3>Añadir nueva ubicación</h3>
    <p>
      <label for="">Nombre de la ubicación</label>
      <input type="text" name="" id="" v-model="name">
    </p>
    
    <p>
      <label for="">Descripción del lugar</label>
      <textarea name="description" id="" cols="30" rows="10" v-model="description"></textarea>
    </p>

    <p>
      <label>Tipo de ubicación</label>

      <select v-model="ubicationType">
        <option v-for="ubication in ubications" :key="ubication">{{ ubication }}</option>
      </select>
    </p>
  <button @click ="addLocation()">Añadir</button>
    
    
  </form>


  <div>
    <p>{{ name }}</p>
    <p>{{ description }}</p>
    <p>{{ ubicationType }}</p>
  </div>
</template>

<script setup>
import { ref } from 'vue';
import jsonData from "../../data.json";

const ubications = ["Playa", "Cala", "Cascada", "Montaña", "Cueva", "Desierto", "Barranco", "Lago", "Río"];
const name = ref("");
const description = ref("");
const ubicationType = ref("");
const data = ref([]);



function addLocation(){
  data.value = {
    name: name.value,
    description: description.value,
    ubicationType: ubicationType.value
  };

  name.value = "";
  description.value = "";
  ubicationType.value = "";

  saveDataToJson();
}


function saveDataToJson() {
   console.log(jsonData);
  
   var dataJson = JSON.stringify(data.value);
  // //-> {"name":"","description":"","ubicationType":""}
    const dataJson2 = JSON.parse(dataJson);
  // // -> {name: '', description: '', ubicationType: ''}
   
  jsonData.push(dataJson2);

  // jsonData = JSON.stringify(jsonData);
  // // Puedes hacer lo que necesites con el JSON, por ejemplo, guardarlo en localStorage
   localStorage.setItem('locations', jsonData);

}
</script>

<style scoped>
</style>