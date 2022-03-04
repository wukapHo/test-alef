<template>
  <section class="personal">
    <h2 class="personal__title">Персональные данные</h2>
    <div class="personal__item">
      <label class="personal__label" for="name">Имя</label>
      <input
        v-model="name"
        @change="saveData"
        class="personal__input"
        id="name"
        type="text"
        placeholder="Введите имя..."
      />
    </div>
    <div class="personal__item">
      <label class="personal__label" for="age">Возраст</label>
      <input
        v-model="age"
        @change="
          {
            validateAge();
            saveData();
          }
        "
        class="personal__input personal__input--number"
        id="age"
        type="number"
        placeholder="Введите возраст..."
      />
    </div>
  </section>
</template>

<script>
export default {
  name: "PersonalData",

  emits: {
    "change-personal": null,
  },

  data() {
    return {
      name: "",
      age: "",
    };
  },

  created() {
    const personalData = localStorage.getItem("personal-data");

    if (personalData) {
      this.name = JSON.parse(personalData).name;
      this.age = JSON.parse(personalData).age;
    }
  },

  methods: {
    validateAge() {
      if (this.age < 18) {
        this.age = 18;
      } else if (this.age > 99) {
        this.age = 99;
      }
    },
    saveData() {
      this.$emit("change-personal", { name: this.name, age: this.age });
    },
  },
};
</script>

<style lang="scss">
.personal {
  max-width: 616px;
  margin: 30px auto;

  &__title {
    margin-bottom: 20px;
    font-weight: 500;
    font-size: 16px;
  }

  &__item {
    width: 100%;
    height: 56px;
    padding: 8px 16px;
    margin-top: 10px;
    display: flex;
    flex-direction: column;
    border: 1px solid #f1f1f1;
    border-radius: 4px;
  }

  &__label {
    font-size: 13px;
    line-height: 16px;
    color: rgba(17, 17, 17, 0.48);
  }

  &__input {
    border: none;
    outline: none;
    -moz-appearance: textfield;

    &--number::-webkit-outer-spin-button,
    &--number::-webkit-inner-spin-button {
      -webkit-appearance: none;
    }
  }
}
</style>
