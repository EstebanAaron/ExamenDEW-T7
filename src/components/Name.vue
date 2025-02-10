<template>
  <div>
    <P>Inserta tu nombre</P>
    <input v-model="name" placeholder="Escribe tu nombre" />
    <p v-if="name.length">Tu nombre {{ name }} tiene: {{ name.length }} letras</p>
  </div>
</template>

<script>
export default {
  data() {
    return {
      name: ''
    };
  }
};
</script>

<style scoped>
input {
  margin-bottom: 10px;
  padding: 5px;
}

p {
  margin: 5px 0;
}
</style>