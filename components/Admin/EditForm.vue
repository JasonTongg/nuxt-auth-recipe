<template>
  <div class="overlay">
    <form class="form">
      <h2>Edit Recipe</h2>
      <div class="inputContainer">
        <label>Recipe Image</label>
        <input
          type="text"
          :placeholder="recipe.recipeImage"
          @change="changeTitle($event)"
          name="recipeImage"
        />
      </div>
      <div class="inputContainer">
        <label>Recipe Title</label>
        <input
          type="text"
          :placeholder="recipe.recipeTitle"
          @change="changeTitle($event)"
          name="recipeTitle"
        />
      </div>
      <div class="inputContainer" v-for="(_, index) in recipe.ingredients">
        <label>Recipe Ingredients</label>
        <input
          type="text"
          :placeholder="recipe.ingredients[index]"
          @change="changeIng($event, index)"
        />
      </div>
      <div class="inputContainer" v-for="(_, index) in recipe.directions">
        <label>Recipe Directions</label>
        <input
          type="text"
          :placeholder="recipe.directions[index]"
          @change="changeDir($event, index)"
        />
      </div>
      <div class="inputContainer">
        <label>Recipe Description</label>
        <textarea
          cols="30"
          rows="10"
          @change="changeTitle($event)"
          :placeholder="recipe.description"
          name="description"
        ></textarea>
      </div>
      <div class="buttonContainer">
        <button class="deleteConfirm" @click="addRecipe">Save</button>
        <button class="editConfirm">Cancel</button>
      </div>
      {{ newRecipe }}
    </form>
  </div>
</template>
<script>
export default {
  props: ["recipe"],
  data() {
    return {
      newRecipe: this.recipe
        ? { ...this.recipe }
        : {
            recipeImage: "",
            recipeTitle: "",
            ingredients: [],
            directions: [],
            description: "",
          },
    };
  },
  methods: {
    addRecipe() {
      let { ...newRecipe } = this.newRecipe;
      this.$store
        .dispatch("updateRecipe", { newRecipe, id: this.newRecipe.id })
        .then(() => {
          this.$router.push("/user");
        });
    },
    changeTitle(event) {
      this.newRecipe = {
        ...this.newRecipe,
        [event.target.name]: event.target.value,
      };
    },
    changeIng(event, index) {
      let news = { ...this.newRecipe };
      news.ingredients[index] = event.target.value;
      this.newRecipe = news;
    },
    changeDir(event, index) {
      let news = { ...this.newRecipe };
      news.directions[index] = event.target.value;
      this.newRecipe = news;
    },
  },
};
</script>
<style scoped>
.overlay {
  position: fixed;
  width: 100vw;
  height: 100vh;
  top: 0;
  left: 0;
  background-color: rgba(0, 0, 0, 0.2);
  display: flex;
  align-items: center;
  justify-content: center;
  z-index: 10;
}

form {
  background-color: white;
  padding: 2rem;
  display: flex;
  align-items: center;
  justify-content: center;
  flex-direction: column;
  border-radius: 20px;
  gap: 1rem;
}

.inputContainer {
  display: grid;
  grid-template-columns: 1fr 3fr;
  justify-items: flex-end;
  gap: 2rem;
  width: 100%;
}

.inputContainer input,
.inputContainer textarea {
  width: 100%;
}

.deleteConfirm {
  background-color: green;
}
.editConfirm {
  background-color: red;
}
</style>
