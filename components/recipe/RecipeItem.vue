<template>
  <div class="card col-md-3">
    <img
      class="recipes-content__img card-img-top rounded"
      :alt="recipe.recipeTitle"
      :src="recipe.recipeImage"
    />
    <p class="username">{{ recipe.username }}</p>
    <div class="card-body">
      <nuxt-link
        class="card-text fs-5 text"
        tag="h1"
        :to="{ name: 'recipe-recipeId', params: { recipeId: recipe.id } }"
        style="height: 45px; align-item: center"
      >
        {{ recipe.recipeTitle }}
      </nuxt-link>
      <div class="recipes-content__body__review card-footer bg-transparent">
        <img :src="likeImage" alt="Heart" @click="likeClick" />
        <p>{{ likeCount }} likes</p>
        <button class="delete" v-show="isIcon === true" @click="overlay(true)">
          <i class="fa fa-trash" aria-hidden="true"></i>
        </button>
        <button
          class="edit"
          v-show="isIcon === true"
          @click="editOverlayFunc(true)"
        >
          <i class="fa fa-file" aria-hidden="true"></i>
        </button>
      </div>
    </div>
    <div class="overlay" v-show="overlayShow === true">
      <div class="overlayContent">
        <h2>Are you sure want to delete this recipe?</h2>
        <div class="buttonContainer">
          <button class="deleteConfirm" @click="deleteRecipe">Delete</button>
          <button class="editConfirm" @click="overlay(false)">Cancel</button>
        </div>
      </div>
    </div>
    <EditForm v-show="editOverlay === true" :recipe="recipe" />
  </div>
</template>
<script>
import EditForm from "../Admin/EditForm.vue";
export default {
  props: ["recipe", "isIcon"],
  data() {
    return {
      email: [],
      overlayShow: false,
      editOverlay: false,
    };
  },
  components: {
    EditForm,
  },
  computed: {
    likeCount() {
      if (this.recipe.dataLikes?.length === 1) {
        if (this.recipe.dataLikes[0] === "null") {
          return 0;
        }
        return 1;
      }
      return this.recipe.dataLikes?.length;
    },
    likeImage() {
      let userEmail = this.$store.getters.userEmail;
      if (!this.recipe.dataLikes.some((item) => item[0] === userEmail)) {
        return "images/heart-black.png";
      }
      return "images/heart-red.png";
    },
  },
  methods: {
    overlay(toggle) {
      this.overlayShow = toggle;
    },
    editOverlayFunc(toggle) {
      this.editOverlay = toggle;
    },
    likeClick() {
      console.log("tes");
      if (!this.$store.getters.isAuthenticated) {
        this.$router.push("/user/login");
      }
      const userEmail = this.$store.getters.userEmail;

      if (this.email?.length === 1 && this.email[0] === "null") {
        this.email[0] = userEmail;
      } else {
        const checkLike = this.email?.filter((item) => item === userEmail);
        if (checkLike?.length === 0) {
          this.email.push(userEmail);
        } else {
          if (this.email?.length === 1) {
            this.email[0] = "null";
          } else {
            const userEmailIndex = this.email?.findIndex(
              (item) => item === userEmail
            );
            this.email?.splice(userEmailIndex, 1);
          }
        }
      }
      let { id, ...newRecipe } = this.recipe;
      if (this.recipe.dataLikes[0] === "null") {
        this.$store.dispatch("likeUpdate", {
          recipeId: this.recipe.id,
          newDataRecipe: {
            ...newRecipe,
            dataLikes: [this.email],
          },
        });
      } else {
        if (!this.recipe.dataLikes.some((item) => item[0] === userEmail)) {
          this.$store.dispatch("likeUpdate", {
            recipeId: this.recipe.id,
            newDataRecipe: {
              ...newRecipe,
              dataLikes: [...this.recipe.dataLikes, this.email],
            },
          });
        }
      }
    },
    deleteRecipe() {
      this.$store.dispatch("deleteRecipe", this.recipe.id);
      this.overlay(false);
    },
  },
};
</script>
<style>
.recipes-content__img {
  width: 100%;
  height: 25vh;
}
.recipes-content__body__review {
  display: flex;
  gap: 10px;
}
.username {
  margin-bottom: 0px;
}
.delete,
.edit {
  width: 30px;
  height: 30px;
  color: white;
  border: none;
  border-radius: 50%;
  font-size: 1rem;
}

.delete {
  background-color: red;
}

.edit {
  background-color: rgb(194, 194, 4);
}

.deleteConfirm,
.editConfirm {
  color: white;
  border: none;
  border-radius: 100px;
  font-size: 1rem;
  padding: 0.5rem 1rem;
}

.deleteConfirm {
  background-color: red;
}

.editConfirm {
  background-color: rgb(194, 194, 4);
}

.overlay {
  position: fixed;
  width: 100vw;
  height: 100vh;
  top: 0;
  left: 0;
  background-color: rgba(0, 0, 0, 0.5);
  display: flex;
  align-items: center;
  justify-content: center;
}

.overlayContent {
  background-color: white;
  padding: 2rem;
  display: flex;
  align-items: center;
  justify-content: center;
  flex-direction: column;
  border-radius: 20px;
}

.buttonContainer {
  display: flex;
  align-items: center;
  justify-content: center;
  gap: 1rem;
}
</style>
