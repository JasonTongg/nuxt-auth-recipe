<template>
  <div class="container">
    <div class="row g-5">
      <RecipeItem
        v-for="data in dataRecipes"
        :key="data.id"
        :recipe="data"
        :isIcon="isIcon"
      ></RecipeItem>
    </div>
  </div>
</template>
<script>
import RecipeItem from "./RecipeItem.vue";
export default {
  components: {
    RecipeItem,
  },
  props: ["dataRecipes", "isIcon"],
};
</script>
<style>
.recipes {
  display: flex;
  flex-wrap: wrap;
}
</style>
