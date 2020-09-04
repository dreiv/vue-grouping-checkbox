<template>
  <div>
    <input type="checkbox" v-model="checked" />
    <div v-for="group of groups" :key="group.id">
      <input type="checkbox" :value="group" v-model="group.checked" />
      {{group.name}}
      <label v-for="item of group.items" :key="item.id">
        <input type="checkbox" :value="item" v-model="item.checked" />
        {{ item.name }}
      </label>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    groups: {
      type: Array,
      required: true,
    },
  },
  data() {
    return {
      checked: false,
    };
  },
};
</script>

<style scoped>
label {
  display: block;
  margin-left: 0.5rem;
}
</style>
