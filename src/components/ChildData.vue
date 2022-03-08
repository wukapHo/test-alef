<template>
  <div class="child">
    <div class="child__input">
      <label :for="'name-' + modelValue.id" class="personal__label">Имя</label>
      <input
        ref="input"
        :id="'name-' + modelValue.id"
        :value="modelValue.name"
        @input="
          $emit('update:modelValue', {
            ...modelValue,
            name: $event.target.value,
          })
        "
        @change="validationName($event.target.value)"
        class="personal__input"
        type="text"
        placeholder="Введите имя..."
      />
    </div>

    <div class="child__input">
      <label :for="'age-' + modelValue.id" class="personal__label">
        Возраст</label
      >
      <input
        :id="'age-' + modelValue.id"
        :value="modelValue.age"
        @input="
          $emit('update:modelValue', {
            ...modelValue,
            age: +$event.target.value,
          })
        "
        @change="validationAge($event.target.value)"
        class="personal__input personal__input--number"
        type="number"
        placeholder="Введите возраст..."
      />
    </div>

    <button @click="deleteChild" class="child__delete-btn">
      <span class="to-be-hidden">Удалить</span>
    </button>
  </div>
</template>

<script>
export default {
  name: "ChildData",

  props: {
    modelValue: {
      type: Object,
      default() {
        return {
          name: "",
          age: null,
          id: null,
        };
      },
    },
  },

  emits: {
    "update:modelValue": null,
    "delete-child": null,
  },

  mounted() {
    this.$refs.input.focus();
  },

  methods: {
    validationName(data) {
      if (data.length > 20) {
        data = data.slice(0, 21);
      }
      this.$emit("update:modelValue", { ...this.modelValue, name: data });
    },
    validationAge(value) {
      if (value < 1) {
        value = 1;
      } else if (value > 99) {
        value = 99;
      }
      this.$emit("update:modelValue", { ...this.modelValue, age: +value });
    },
    deleteChild() {
      this.$emit("delete-child", this.modelValue);
    },
  },
};
</script>

<style lang="scss">
.child {
  width: 100%;
  margin-top: 10px;
  display: flex;
  align-items: center;

  &__input {
    width: 42%;
    height: 56px;
    padding: 8px 16px;
    margin-right: 18px;
    display: flex;
    flex-direction: column;
    border: 1px solid #f1f1f1;
    border-radius: 4px;

    @media (max-width: 420px) {
      margin-right: 0;
    }
  }

  &__delete-btn {
    height: 56px;
    min-width: 56px;
    border: none;
    border-radius: 4px;
    outline: none;
    background: transparent;
    color: #01a7fd;
    cursor: pointer;
    opacity: 0.7;
    transition: 0.3s;

    @media (max-width: 768px) {
      background: url("../assets/plus-icon.svg") no-repeat center;
      background-size: 60%;
      transform: rotate(45deg);

      .to-be-hidden {
        display: none;
      }
    }

    &:hover {
      opacity: 1;
    }

    &:active {
      transform: scale(1.1);
    }

    .to-be-hidden {
      color: #01a7fd;
    }
  }
}
</style>
