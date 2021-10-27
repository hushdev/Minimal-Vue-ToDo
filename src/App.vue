<template>
  <div class="container">
    <app-navbar />
    <app-categories />
  </div>
</template>

<script>
import AppNavbar from "@/components/AppNavbar";
import AppCategories from "@/components/AppCategories";

export default {
  components: { AppNavbar, AppCategories },
};
</script>

<style lang="scss">
@import "@/assets/scss/index.scss";
</style>
