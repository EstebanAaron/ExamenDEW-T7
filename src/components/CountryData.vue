<template>
  <div v-if="country">
    <table>
      <tr>
      <th>Name</th>
      <td>{{ country.name }}</td>
      </tr>
      <tr>
      <th>PIB per capita</th>
      <td>${{ (country.pib / country.population).toFixed(2) }}</td>
      </tr>
      <tr>
      <th>Population</th>
      <td>{{ country.population }} million</td>
      </tr>
      <tr>
      <th>Income</th>
      <td>${{ country.income }} per capita</td>
      </tr>
      <tr>
      <th>PIB</th>
      <td>${{ country.pib }} million</td>
      </tr>
      <tr>
      <th>Area</th>
      <td>{{ country.area }} km²</td>
      </tr>
    </table>
    <button @click="$emit('clearCode')">Eliminar</button>
  </div>
  
  <div v-else>
    <p v-if="error">{{ error }}</p>
    <p v-else>Loading...</p>
  </div>
</template>

<script>
export default {
  props: {
    code: {
      type: String,
      required: true
    }
  },
  data() {
    return {
      country: null,
      error: null
    };
  },
  watch: {
    code: 'fetchCountryData'
  },
  created() {
    this.fetchCountryData();
  },
  methods: {
    async fetchCountryData() {
      this.country = null;
      this.error = null;
      try {
        const response = await fetch(`http://localhost:3000/api/country/${this.code}`);
        if (!response.ok) {
              this.$emit('removeFromSelecteds', this.code);
          throw new Error('No se encontró ese país');
        }
        const data = await response.json();
        this.country = data;
      } catch (error) {
        this.error = error.message;
      }
    }
  }
};
</script>

<style scoped>
/* Add your styles here */
</style>