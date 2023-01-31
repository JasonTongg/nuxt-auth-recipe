<template>
  <div>
    <header class="header">
      <h1 class="header-title">Food Recipes</h1>
      <nav class="header-nav">
        <nuxtLink tag="button" to="/" class="header-nav__link"> Home </nuxtLink>
        <nuxtLink tag="button" to="/add" class="header-nav__link">
          Add Recipes
        </nuxtLink>
        <nuxt-link
          tag="a"
          to="/user/login"
          class="header-nav__link"
          v-if="!$store.getters.isAuthenticated"
        >
          Login
        </nuxt-link>
        <a
          class="header-nav__link"
          v-if="$store.getters.isAuthenticated"
          @click="logout"
        >
          Logout
        </a>
        <a class="header-nav__link" v-if="$store.getters.isAuthenticated">
          {{ $store.state.userData.username }}
        </a>
      </nav>
    </header>
  </div>
</template>
<script>
export default {
  methods: {
    logout() {
      this.$store.commit("setToken", null);
      this.$router.push("/");
    },
  },
};
</script>
<style>
.body {
  margin: 0px;
}

header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding-left: 40px;
  padding-right: 40px;
  background-color: #eef2e6;
}

main {
  margin: 22px;
}

.recipes {
  display: flex;
  flex-wrap: wrap;
}

.header-nav__link {
  margin-left: 20px;
  color: black;
  text-decoration: none;
  font-size: 20px;
}
</style>
