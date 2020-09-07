<template>
  <div>
    <input
      type="checkbox"
      v-model="checked"
      v-indeterminate="indeterminate"
      @change="updateAll(checked)"
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
          @change="updateItem(group)"
        />
        {{ item.name }}
      </label>
    </div>
  </div>
</template>

<script>
import Vue from "vue";
const isChecked = ({ checked }) => checked;

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
      this.updateItem(group)
    });
  },
  methods: {
    updateAll(checked) {
      this.groups.forEach((group) => {
        this.indeterminate = false;
        group.checked = checked;

        group.items.forEach((item) => {
          group.indeterminate = false;
          item.checked = checked;
        });
      });
    },
    updateTop() {
      if (this.groups.some(({ indeterminate }) => indeterminate)) {
        return (this.indeterminate = true);
      }
      const every = this.groups.every(isChecked);
      const some = this.groups.some(isChecked);

      this.checked = every;
      this.indeterminate = !every && every !== some;
    },
    updateGroup(group, checked) {
      const { items } = group;

      group.indeterminate = false;
      items.forEach((item) => {
        Vue.set(item, "checked", checked);
      });

      this.updateTop();
    },
    updateItem(group) {
      const { items } = group;
      const every = items.every(isChecked);
      const some = items.some(isChecked);

      Vue.set(group, "checked", every);
      Vue.set(group, "indeterminate", !every && every !== some);

      this.updateTop();
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
