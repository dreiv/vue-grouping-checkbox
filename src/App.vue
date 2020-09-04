<template>
  <div id="app">
    <checkbox-groups :groups="groups"/>

    <hr>
    <p>Items: {{ groups }}</p>
  </div>
</template>

<script>
import CheckboxGroups from "@/components/CheckboxGroups.vue";

export default {
  name: "App",
  components: { CheckboxGroups },
  data() {
    return {
      groups: [
        {
          name: "Group One",
          id: 1,
          items: [
            { name: "Item One", id: 1 },
            { name: "Item Two", id: 2 },
            { name: "Item Three", id: 3 },
          ],
        },
        {
          name: "Group Two",
          id: 2,
          items: [
            { name: "Item Four", id: 4 },
            { name: "Item Five", id: 5 },
            { name: "Item Six", id: 6 },
          ],
        },
      ],
    };
  },
};
</script>
