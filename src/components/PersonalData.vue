<template>
  <section class="personal">
    <h2 class="personal__title">Персональные данные</h2>
    <div class="personal__item">
      <label class="personal__label" for="name">Имя</label>
      <input
        :value="modelValue.name"
        ref="input"
        @input="
          $emit('update:modelValue', {
            ...modelValue,
            name: $event.target.value,
          })
        "
        @change="validationName($event.target.value)"
        class="personal__input"
        id="name"
        type="text"
        placeholder="Введите имя..."
      />
    </div>
    <div class="personal__item">
      <label class="personal__label" for="age">Возраст</label>
      <input
        :value="modelValue.age"
        @input="
          $emit('update:modelValue', {
            ...modelValue,
            age: +$event.target.value,
          })
        "
        @change="validationAge($event.target.value)"
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

  props: {
    modelValue: {
      type: Object,
      default() {
        return {
          name: "",
          age: null,
        };
      },
    },
  },

  emits: {
    "update:modelValue": null,
  },

  mounted() {
    if (this.$refs.input.value === "") {
      this.$nextTick().then(() => {
        this.$refs.input.focus();
      });
    }
  },

  methods: {
    validationName(data) {
      if (data.length > 20) {
        data = data.slice(0, 21);
      }
      this.$emit("update:modelValue", { ...this.modelValue, name: data });
    },
    validationAge(data) {
      if (data < 1) {
        data = 1;
      } else if (data > 99) {
        data = 99;
      }
      this.$emit("update:modelValue", { ...this.modelValue, age: +data });
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
