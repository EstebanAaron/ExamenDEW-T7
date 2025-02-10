<script setup>
import Name from './components/Name.vue'
import Codes from './components/Codes.vue'
import Selected from './components/Selected.vue'
import CountryData from './components/CountryData.vue'
import GoogleChart from './components/GoogleChart.vue'

import { ref,computed } from 'vue'
const selectedCodes = ref([]);

const selectedCode = ref(null);

const dataTypes = ['population', 'pib', 'area', 'income'];
const selectedDataType = ref('population');

function CodeSelect(code) {
  if (!selectedCodes.value.includes(code)) {
    selectedCodes.value.push(code);
  }
  selectedCode.value = code;
}
function removeFromSelecteds(code) {
  const index = selectedCodes.value.indexOf(code);
  if (index > -1) {
    selectedCodes.value.splice(index, 1);
  }
  if (selectedCode.value === code) {
    selectedCode.value = null;
  }
}


const clearCode = async () => {
  if (selectedCode.value) {
    try {
      const response = await fetch(`http://localhost:3000/api/country/${selectedCode.value}`, {
        method: 'DELETE'
      });
      if (!response.ok) {
        throw new Error('Error al eliminar el país');
      }
      // Eliminar el código del array selectedCodes
      removeFromSelecteds(selectedCode.value);
      selectedCode.value = null;
    } catch (error) {
      console.error('Error al eliminar el país:', error);
    }
  }
}
const chartData = computed(() => {
  const data = [[selectedCodes.value, selectedDataType.value]];
  selectedCodes.value.forEach(code => {
    // Aquí debes obtener el nombre del país y el dato correspondiente
    // Supongamos que tienes una función getCountryData que devuelve los datos del país
    const country = getCountryData(code);
    if (country) {
      data.push([country.name, country[selectedDataType.value]]);
    }
  });
  console.log(data);
  
  return data;
});

function getCountryData(code) {
  // Esta función debe devolver los datos del país basado en el código
  // Aquí puedes implementar la lógica para obtener los datos del país
  // Por simplicidad, se devuelve un objeto de ejemplo
  const countries = {
    'EG': { name: 'Egipto', population: 102, pib: 250, area: 1002450, income: 3000 },
    'DE': { name: 'Alemania', population: 83, pib: 4000, area: 357022, income: 45000 },
    'ES': { name: 'España', population: 47, pib: 1500, area: 505992, income: 35000 }
  };
  return countries[code];
}




</script>

<template>
  <div class="parent">
    <div class="header">
      <h1>Datos mundiales</h1>
    </div>
    <div class="name">
      <Name />
    </div>
    <div class="div-codes">
      <Codes
        @CodeSelect="CodeSelect" 
        @removeAllCart="cartIds = []"     
      />
    </div>
    <div class="selected">
      <Selected
        :codes="selectedCodes"
        @remove-code="index => selectedCodes.splice(index, 1)"
      />
    </div>
    <div class="country-data">

      <CountryData 
      :code="selectedCode"
      @clearCode="clearCode"
      @removeFromSelecteds="removeFromSelecteds"
      />
    </div>
    <div class="options">
      <h2>Options</h2>
      <div>
        <label v-for="(type, index) in dataTypes" :key="index">
          <input type="radio" name="radio" :value="type" v-model="selectedDataType" />
          {{ type }}
        </label>
      </div>
    </div>
    <div class="chart">
      <GoogleChart :data="chartData" />
    </div>
  </div>
</template>

<style scoped>
.parent {
  display: grid;
  grid-template-columns: repeat(5, 1fr);
  grid-template-rows: auto repeat(3, 1fr);
  grid-column-gap: 0px;
  grid-row-gap: 0px;
  height: 100vh;
  margin: -2rem;
}

.header {
  grid-area: 1 / 1 / 2 / 6;
  background-color: aquamarine;
}

.header h1 {
  margin: 2px;
}

.div-codes {
  grid-area: 2 / 1 / 6 / 2;
  background-color: azure;
  overflow-y: auto;
}

.name {
  grid-area: 2 / 2 / 3 / 3;
  background-color: antiquewhite;
}

.selected {
  grid-area: 2 / 5 / 4 / 6;
  background-color: lightyellow;
  overflow-y: auto;
}

.country-data {
  grid-area: 2 / 3 / 3 / 5;
  background-color: lightseagreen;
}

.options {
  grid-area: 3 / 2 / 4 / 5;
  background-color: bisque;
}

.chart {
  grid-area: 4 / 2 / 5 / 6;
  background-color: aqua;
}

ul li {
  text-align: left;
  font-size: 0.9rem;
}

ul li:hover {
  font-weight: bold;
  cursor: pointer
}

ul li button {
  font-size: 0.6rem;
  margin: 2px;
}

h2 {
  font-size: 1.2rem;
}
</style>