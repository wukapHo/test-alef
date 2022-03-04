<template>
  <personal-data v-model="personalData" />
  <section class="children">
    <div class="children__wrapper">
      <h2 class="children__title">Дети (макс. 5)</h2>
      <button
        ref="add"
        v-if="childrenData.length < maxChildren"
        @click="addChild"
        class="children__add-btn"
      >
        Добавить ребенка
      </button>
    </div>
    <child-data
      v-for="(child, idx) in childrenData"
      :key="child.id"
      @delete-child="deleteChild($event)"
      v-model="childrenData[idx]"
    />
  </section>
  <div class="save">
    <button class="save__btn">Сохранить</button>
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
      personalData: { name: "", age: "" },
      childrenData: [],
      maxChildren: 5,
    };
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

    if (
      this.childrenData.some((item) => item.name === "" || item.age === null)
    ) {
      this.$nextTick().then(() => {
        this.$refs.add.disabled = true;
      });
    }
  },

  methods: {
    addChild() {
      const id = Math.floor(Math.random() * 9999);
      this.childrenData = [
        ...this.childrenData,
        { name: "", age: null, id: id },
      ];

      const length = this.childrenData.length;
      if (
        length > 0 &&
        (this.childrenData[length - 1].name === "" ||
          this.childrenData[length - 1].age === "")
      ) {
        this.$refs.add.disabled = true;
        return;
      }
    },
    deleteChild(item) {
      this.childrenData = this.childrenData.filter(
        (child) => child.id !== item.id
      );
      this.updateButton();
    },
    updateButton() {
      const length = this.childrenData.length;
      if (length === 5) return;
      if (
        this.childrenData.every(
          (child) => child.name !== "" && child.age !== null
        )
      ) {
        this.$nextTick().then(() => {
          this.$refs.add.disabled = false;
        });
      }
    },
  },

  watch: {
    personalData() {
      localStorage.setItem("personal-data", JSON.stringify(this.personalData));
    },
    childrenData: {
      deep: true,

      handler() {
        this.updateButton();
        localStorage.setItem(
          "children-data",
          JSON.stringify(this.childrenData)
        );
      },
    },
  },
};
</script>

<style lang="scss">
.children {
  max-width: 616px;
  margin: 0 auto;

  &__wrapper {
    height: 44px;
    display: flex;
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
    border: 2px solid #01a7fd;
    border-radius: 100px;
    background: transparent url("../assets/plus-icon.svg") no-repeat center left
      15px;
    color: #01a7fd;
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
