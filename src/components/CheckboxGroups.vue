<template>
  <div>
    <input type="checkbox" v-model="checked" />
    <div v-for="group of groups" :key="group.id">
      <input
        type="checkbox"
        :value="group"
        v-model="group.checked"
        v-indeterminate="indeterminate"
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
    indeterminate(el, binding) {
      el.indeterminate = Boolean(binding.value);
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
  },
};
</script>

<style scoped>
label {
  display: block;
  margin-left: 0.5rem;
}
</style>
