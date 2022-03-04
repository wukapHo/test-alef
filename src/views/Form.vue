<template>
  <personal-data @change-personal="savePersonal($event)" />
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

  methods: {
    savePersonal(data) {
      localStorage.setItem("personal-data", JSON.stringify(data));
    },
    saveChildren(data) {
      localStorage.setItem("children-data", JSON.stringify(data));
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
