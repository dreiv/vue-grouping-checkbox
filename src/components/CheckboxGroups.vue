<template>
  <div>
    <input type="checkbox" v-model="checked" />
    <div v-for="group of groups" :key="group.id">
      <input
        type="checkbox"
        :value="group"
        v-model="group.checked"
        v-indeterminate="group.indeterminate"
        @change="updateChildren(group, group.checked)"
      />
      {{group.name}}
      <label v-for="item of group.items" :key="item.id">
        <input
          type="checkbox"
          :value="item"
          v-model="item.checked"
          @change="updateGroup(group)"/>
        {{ item.name }}
      </label>
    </div>
  </div>
</template>

<script>
import Vue from "vue";

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
  directives: {
    indeterminate: {
      update(el, binding) {
        el.indeterminate = Boolean(binding.value);
      },
    },
  },
  methods: {
    updateGroup(group) {
      const { items } = group;
      const every = items.every(({ checked }) => checked);
      const some = items.some(({ checked }) => checked);

      group.checked = every;
      group.indeterminate = !every && every !== some;
    },
    updateChildren(group, checked) {
      const { items } = group;

      group.indeterminate = false;
      items.forEach((item) => {
        Vue.set(item, "checked", checked);
      });
    },
  },
};
</script>

<style scoped>
label {
  display: block;
  margin-left: 0.5rem;
}
</style>
