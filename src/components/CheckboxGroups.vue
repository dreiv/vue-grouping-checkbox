<template>
  <div>
    <input
      type="checkbox"
      v-model="checked"
      v-indeterminate="indeterminate"
      @change="update(checked)"
    />
    <div v-for="group of groups" :key="group.id">
      <input
        type="checkbox"
        :value="group"
        v-model="group.checked"
        v-indeterminate="group.indeterminate"
        @change="updateGroup(group, group.checked)"
      />
      {{group.name}}

      <label v-for="item of group.items" :key="item.id">
        <input
          type="checkbox"
          :value="item"
          v-model="item.checked"
          @change="updateChild(group)"
        />
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
      indeterminate: false,
    };
  },
  directives: {
    indeterminate: {
      update(el, binding) {
        el.indeterminate = Boolean(binding.value);
      },
    },
  },
  mounted() {
    this.groups.forEach((group) => {
      group.items.forEach(() => {
        this.updateChild(group);
      });
    });
  },
  methods: {
    update(checked) {
      this.groups.forEach((group) => {
        this.indeterminate = false;
        group.checked = checked;

        group.items.forEach((item) => {
          group.indeterminate = false;
          item.checked = checked;
        });
      });
    },
    updateGroup(group, checked) {
      const { items } = group;

      group.indeterminate = false;
      items.forEach((item) => {
        Vue.set(item, "checked", checked);
        item.checked = checked;
      });
      this.updateTop();
    },
    updateChild(group) {
      const { items } = group;
      const every = items.every(({ checked }) => checked);
      const some = items.some(({ checked }) => checked);

      group.checked = every;
      group.indeterminate = !every && every !== some;
      this.updateTop();
    },
    updateTop() {
      const isIndeterminate = this.groups.some(
        ({ indeterminate }) => indeterminate
      );
      if (isIndeterminate) {
        return (this.indeterminate = true);
      }
      const every = this.groups.every(({ checked }) => checked);
      const some = this.groups.some(({ checked }) => checked);

      this.checked = every;
      this.indeterminate = !every && every !== some;
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
