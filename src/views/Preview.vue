<template>
  <div class="preview">
    <div>Введите данные во вкладке "Форма"</div>
    <div v-if="personalData.name !== '' && personalData.age !== null">
      <h2 class="preview__title">Персональные данные</h2>
      <p class="preview__desc">
        {{ personalData.name }}, {{ normalizeAge(personalData.age) }}
      </p>
    </div>
    <div v-if="normalizedChildrenData.length > 0">
      <h2 class="preview__title preview__title--bottom">Дети</h2>
      <div class="preview__children">
        <div
          v-for="(c, idx) in normalizedChildrenData"
          :key="idx"
          class="preview__child preview__desc"
        >
          {{ normalizedChildrenData[idx].name }},
          {{ normalizeAge(normalizedChildrenData[idx].age) }}
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "Preview",

  data() {
    return {
      personalData: null,
      childrenData: [],
    };
  },

  computed: {
    normalizedChildrenData() {
      return this.childrenData.filter(
        (item) => item.name !== "" && item.age !== null
      );
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
  },

  methods: {
    normalizeAge(value) {
      if (value % 100 >= 5 && value % 100 <= 20) {
        return `${value} лет`;
      }

      if (value % 10 === 1) {
        return `${value} год`;
      }

      if (value % 10 >= 2 && value % 10 <= 4) {
        return `${value} года`;
      }

      return `${value} лет`;
    },
  },
};
</script>

<style lang="scss">
.preview {
  max-width: 616px;
  margin: 0 auto;
  padding-top: 30px;

  &__title {
    margin-bottom: 20px;
    font-size: 16px;
    font-weight: 500;

    &--bottom {
      margin-top: 60px;
      margin-bottom: 0;
    }
  }

  &__desc {
    font-size: 16px;
    font-weight: 700;
  }

  &__children {
    display: inline-flex;
    flex-direction: column;
  }

  &__child {
    height: 44px;
    padding: 10px 20px;
    margin-top: 20px;
    display: inline-block;
    background: #f1f1f1;
    border-radius: 5px;
  }
}
</style>
