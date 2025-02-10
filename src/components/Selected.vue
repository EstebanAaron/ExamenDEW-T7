<template>
  <div>
    <div v-if="codes.length > 0">
      <h2 > Seleccionados : {{ codes.length }}</h2>
    <ul>
      <li v-for="(code, index) in codesWithNames" :key="index">
        <span v-if="code">{{ code.name }}</span>
        <button @click="removeCode(index)">Desmarcar</button>
      </li>
    </ul>
    </div>
    <p v-else>Debes seleccionar un país</p>
  </div>
</template>

<script>
export default {
  name: 'Selected',
  props: {
    codes: {
      type: Array,
      required: true
    }
  },
  data() {
    return {
      names: {}
    };
  },
  computed: {
    codesWithNames() {
      return this.codes.map(code => ({
        code,
        name: this.names[code] || 'Unknown'
      }));
    }
  },
  watch: {
    codes: {
      handler: 'fetchNames',
      immediate: true
    }
  },
  methods: {
    async fetchNames() {
      try {
        const response = await fetch("http://localhost:3000/api/names");
        const data = await response.json();
        this.names = data;
      } catch (error) {
        console.error("Error fetching names:", error);
      }
    },
    removeCode(index) {
      this.$emit('remove-code', index);
    }
  }
}
</script>

<style scoped>
/* Add your styles here */
</style>