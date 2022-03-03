<template>
  <personal-data />
  <children-data />
</template>

<script>
import PersonalData from "../components/PersonalData.vue";
import ChildrenData from "../components/ChildrenData.vue";

export default {
  name: "Form",

  components: {
    PersonalData,
    ChildrenData,
  },
};
</script>
