<template>
  <div class="line"></div>
  <div class="container">
    <app-header />
    <router-view />
  </div>
  <footer class="footer">all rights reserved</footer>
</template>

<script>
import AppHeader from "./components/AppHeader.vue";

export default {
  name: "App",

  components: {
    AppHeader,
  },
};
</script>

<style lang="scss">
@import url("https://fonts.googleapis.com/css2?family=Montserrat:wght@400;500;700&display=swap");

* {
  padding: 0;
  margin: 0;
  box-sizing: border-box;
  list-style: none;
  font-family: "Montserrat", sans-serif;
  font-weight: 400;
  font-size: 14px;
  line-height: 24px;
  color: #111111;
  text-decoration: none;
}

.line {
  width: 100%;
  height: 76px;
  position: absolute;
  box-shadow: 0px 1px 0px rgba(17, 17, 17, 0.1);
}

.container {
  max-width: 1366px;
  min-height: calc(100vh - 76px);
  padding: 0 20px;
  margin: 0 auto;
}

.footer {
  width: 100%;
  height: 76px;
  display: flex;
  align-items: center;
  justify-content: center;
  background: #fafafa;
  box-shadow: 0px 1px 0px rgba(17, 17, 17, 0.1);
}
</style>
