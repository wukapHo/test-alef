<template>
  <section class="children">
    <div class="children__wrapper">
      <h2 class="children__title">Дети (макс. 5)</h2>
      <button
        ref="add"
        v-if="childrenList.length < 5"
        @click="addChild"
        class="children__add-btn"
      >
        Добавить ребенка
      </button>
    </div>
    <div class="children__field">
      <div v-for="(c, idx) in childrenList" :key="idx" class="children__item">
        <div class="children__item-input">
          <label :for="'name-' + `${idx}`" class="personal__label">Имя</label>
          <input
            v-model="childrenList[idx].name"
            :id="'name-' + `${idx}`"
            @change="saveData()"
            @input="updateButton"
            class="personal__input"
            type="text"
            placeholder="Введите имя..."
          />
        </div>
        <div class="children__item-input">
          <label :for="'age-' + `${idx}`" class="personal__label">
            Возраст</label
          >
          <input
            v-model="childrenList[idx].age"
            :id="'age-' + `${idx}`"
            @change="
              {
                validateAge(idx);
                saveData();
              }
            "
            @input="updateButton"
            class="personal__input personal__input--number"
            type="number"
            placeholder="Введите возраст..."
          />
        </div>
        <button
          @click="deleteChild(childrenList[idx])"
          class="children__item-delete-btn"
        >
          Удалить
        </button>
      </div>
    </div>
  </section>
</template>

<script>
export default {
  name: "ChildrenData",

  emits: {
    "change-children": null,
  },

  data() {
    return {
      childrenList: [],
    };
  },

  created() {
    const childrenData = localStorage.getItem("children-data");

    if (childrenData) {
      this.childrenList = JSON.parse(childrenData);
    }
  },

  methods: {
    addChild() {
      this.childrenList.push({ name: "", age: "" });
      const length = this.childrenList.length;
      if (
        length > 0 &&
        (this.childrenList[length - 1].name === "" ||
          this.childrenList[length - 1].age === "")
      ) {
        this.$refs.add.disabled = true;
        return;
      }
    },
    deleteChild(child) {
      const length = this.childrenList.length;
      this.childrenList = this.childrenList.filter((c) => c !== child);
      if (
        length === 0 ||
        this.childrenList.every((c) => c.name !== "" && c.age !== "")
      ) {
        this.$nextTick().then(() => {
          this.$refs.add.disabled = false;
        });
      }
    },
    validateAge(idx) {
      if (this.childrenList[idx].age < 1) {
        this.childrenList[idx].age = 1;
      } else if (this.childrenList[idx].age > 99) {
        this.childrenList[idx].age = 99;
      }
    },
    updateButton() {
      const length = this.childrenList.length;
      if (length === 5) return;
      if (
        this.childrenList[length - 1].name !== "" &&
        this.childrenList[length - 1].age !== ""
      ) {
        this.$refs.add.disabled = false;
      }
    },
    saveData() {
      this.$emit("change-children", this.childrenList);
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

  &__item {
    width: 100%;
    margin-top: 10px;
    display: flex;
    align-items: center;
  }

  &__item-input {
    width: 42%;
    height: 56px;
    padding: 8px 16px;
    margin-right: 18px;
    display: flex;
    flex-direction: column;
    border: 1px solid #f1f1f1;
    border-radius: 4px;
  }

  &__item-delete-btn {
    height: 56px;
    border: none;
    border-radius: 4px;
    outline: none;
    background: transparent;
    color: #01a7fd;
    cursor: pointer;
    opacity: 0.7;
    transition: 0.3s;

    &:hover {
      opacity: 1;
    }

    &:active {
      transform: scale(1.1);
    }
  }
}
</style>
