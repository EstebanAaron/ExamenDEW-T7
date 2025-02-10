<script>
import codes from "../data/codes.json";

export default {
  data() {
    return {
      codes,
      names: {}
    };
  },
  created() {
    this.fetchNames();
  },
  methods: {
    async fetchNames() {
      const response = await fetch("http://localhost:3000/api/names");
      const data = await response.json();
      this.names = data;
    },
    handleClick(code) {
      this.$emit('CodeSelect', code);
      console.log("Code clicked:", code);
    }
  },
  computed: {
    codesWithNames() {
      return this.codes
        .map(code => ({
          code,
          name: this.names[code] || 'Unknown'
        }))
        .sort((a, b) => a.name.localeCompare(b.name));
    }
  }
};
</script>

<template>
  <div>
    <ul>
      <li v-for="item in codesWithNames" @click="handleClick(item.code)">
        {{ item.name }}
      </li>
    </ul>
  </div>
</template>

<style scoped></style>