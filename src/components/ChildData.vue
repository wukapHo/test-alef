<template>
  <div class="child">
    <div class="child__input">
      <label :for="'name-' + id" class="personal__label">Имя</label>
      <input
        :id="'name-' + id"
        :value="modelValue.name"
        @input="updateName($event.target.value)"
        class="personal__input"
        type="text"
        placeholder="Введите имя..."
      />
    </div>
    <div class="child__input">
      <label :for="'age-' + id" class="personal__label">Возраст</label>
      <input
        :id="'age-' + id"
        :value="modelValue.age"
        @change="updateAge($event.target.value)"
        class="personal__input personal__input--number"
        type="number"
        placeholder="Введите возраст..."
      />
    </div>
    <button @click="deleteChild" class="child__delete-btn">Удалить</button>
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
        };
      },
    },
  },

  emits: {
    "update:modelValue": null,
    "delete-child": null,
  },

  data() {
    return {
      id: null,
    };
  },

  methods: {
    updateName(value) {
      this.$emit("update:modelValue", { ...this.modelValue, name: value });
    },
    updateAge(value) {
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
  }

  &__delete-btn {
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
