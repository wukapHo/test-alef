<template>
  <div class="preview">
    <div v-if="personalData">
      <h2 class="preview__title">Персональные данные</h2>
      <p class="preview__desc">
        {{ personalData.name }}, {{ personalData.age }} лет
      </p>
    </div>
    <div>
      <h2 class="preview__title preview__title--bottom">Дети</h2>
      <div class="preview__children">
        <div class="preview__child preview__desc">Петр, 14 лет</div>
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
    };
  },

  created() {
    const personalData = localStorage.getItem("personal-data");

    if (personalData) {
      this.personalData = JSON.parse(personalData);
    }
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
