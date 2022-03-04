<template>
  <personal-data v-model="personalData" />
  <children-data @change-children="saveChildren($event)" />
  <div class="save">
    <button class="save__btn">Сохранить</button>
  </div>
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

  data() {
    return {
      personalData: { name: "", age: "" },
      childrenData: [],
    };
  },

  created() {
    const personalData = localStorage.getItem("personal-data");

    if (personalData) {
      this.personalData.name = JSON.parse(personalData).name;
      this.personalData.age = JSON.parse(personalData).age;
    }
  },

  methods: {
    saveChildren(data) {
      localStorage.setItem("children-data", JSON.stringify(data));
    },
  },

  watch: {
    personalData() {
      localStorage.setItem("personal-data", JSON.stringify(this.personalData));
    },
  },
};
</script>

<style lang="scss">
.save {
  max-width: 616px;
  margin: 30px auto;

  &__btn {
    width: 118px;
    height: 44px;
    background: #01a7fd;
    border: none;
    border-radius: 100px;
    outline: none;
    color: #ffffff;
    cursor: pointer;
    transition: 0.3s;

    &:hover {
      background: #0f79af;
    }

    &:active {
      transform: scale(1.05);
    }
  }
}
</style>
