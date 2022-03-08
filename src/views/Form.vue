<template>
  <personal-data v-model="personalData" />

  <section class="children">
    <div class="children__wrapper">
      <h2 class="children__title">Дети (макс. {{ maxChildren }})</h2>
      <button
        v-if="childrenData.length < maxChildren"
        :disabled="isDisabled"
        :title="title"
        @click="addChild"
        class="children__add-btn"
      >
        Добавить ребенка
      </button>
    </div>

    <child-data
      v-for="(child, idx) in childrenData"
      :key="child.id"
      v-model="childrenData[idx]"
      @delete-child="deleteChild($event)"
    />
  </section>

  <div class="save">
    <button @click="reset" class="save__btn">Сбросить</button>
  </div>
</template>

<script>
import PersonalData from "../components/PersonalData.vue";
import ChildData from "../components/ChildData.vue";

export default {
  name: "Form",

  components: {
    PersonalData,
    ChildData,
  },

  data() {
    return {
      personalData: null,
      personalDataDefault: { name: "", age: null },
      childrenData: null,
      childrenDataDefault: [],
      maxChildren: 5,
    };
  },

  computed: {
    isDisabled() {
      if (this.childrenData.length === 0) return false;

      const { name, age } = this.childrenData[this.childrenData.length - 1];
      if (!name || !age) return true;

      return false;
    },
    title() {
      if (this.childrenData.length) {
        const { name, age } = this.childrenData[this.childrenData.length - 1];
        if (!name) {
          return "Введите имя ребёнка";
        } else if (!age) {
          return "Введите возраст ребёнка";
        }
      }
      return "";
    },
  },

  watch: {
    personalData() {
      localStorage.setItem("personal-data", JSON.stringify(this.personalData));
    },
    childrenData: {
      deep: true,

      handler() {
        localStorage.setItem(
          "children-data",
          JSON.stringify(this.childrenData)
        );
      },
    },
  },

  created() {
    const personalData = localStorage.getItem("personal-data");
    const childrenData = localStorage.getItem("children-data");

    if (personalData) {
      this.personalData = JSON.parse(personalData);
    }
    if (childrenData) {
      this.childrenData = JSON.parse(childrenData);
    }

    if (!this.personalData) {
      this.personalData = { ...this.personalDataDefault };
    }
    if (!this.childrenData) {
      this.childrenData = [...this.childrenDataDefault];
    }
  },

  methods: {
    addChild() {
      const id = Math.floor(Math.random() * 9999);
      this.childrenData = [...this.childrenData, { name: "", age: null, id }];
    },
    deleteChild(item) {
      this.childrenData = this.childrenData.filter(
        (child) => child.id !== item.id
      );
    },
    reset() {
      this.personalData = { ...this.personalDataDefault };
      this.childrenData = [...this.childrenDataDefault];
      localStorage.removeItem("personal-data");
      localStorage.removeItem("children-data");
    },
  },
};
</script>

<style lang="scss">
.children {
  margin: 0 auto;
  max-width: 616px;

  &__wrapper {
    display: flex;
    height: 44px;
    align-items: center;
    justify-content: space-between;
  }

  &__title {
    font-weight: 500;
    font-size: 16px;
  }

  &__add-btn {
    width: 204px;
    height: 44px;
    padding-left: 30px;
    color: #01a7fd;
    border: 2px solid #01a7fd;
    border-radius: 100px;
    background: transparent url("../assets/plus-icon.svg") no-repeat center left
      15px;
    cursor: pointer;
    transition: 0.3s;

    &:hover {
      background-color: rgba(110, 65, 226, 0.04);
    }

    &:active {
      transform: scale(1.05);
    }

    &:disabled {
      opacity: 0.56;
    }
  }
}

.save {
  margin: 30px auto;
  max-width: 616px;

  &__btn {
    width: 118px;
    height: 44px;
    color: #ffffff;
    border: none;
    border-radius: 100px;
    outline: none;
    background: #01a7fd;
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
