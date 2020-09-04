<template>
  <div id="app">
    <label v-for="item of items" :key="item.id">
      <input type="checkbox" :value="item" v-model="item.checked" />
      {{ item.name }}
    </label>
    <hr>
    <p>Items: {{ items }}</p>
  </div>
</template>

<script>
export default {
  name: "App",
  data() {
    return {
      items: [
        { name: "One", id: 1 },
        { name: "Two", id: 2 },
        { name: "Three", id: 3 },
        { name: "Four", id: 4 },
        { name: "Five", id: 5 },
        { name: "Six", id: 6 },
      ],
    };
  },
};
</script>

<style>
label {
  display: block;
}
</style>
