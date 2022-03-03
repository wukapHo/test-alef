<template>
  <section class="personal">
    <h2 class="personal__title">Персональные данные</h2>
    <div class="personal__item">
      <label class="personal__label" for="name">Имя</label>
      <input
        v-model="name"
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
        @change="validateAge"
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

  data() {
    return {
      name: "",
      age: null,
    };
  },

  methods: {
    validateAge() {
      if (this.age < 18) {
        this.age = 18;
      } else if (this.age > 99) {
        this.age = 99;
      }
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
