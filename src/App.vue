<template>
  <div id="app">
    <div id="nav">
      <nav class="navbar navbar-expand-lg navbar-light bg-light">
        <router-link class="navbar-brand" :to="{ name: 'main' }"
          ><b>Peachy Recipes</b></router-link
        >|
        <router-link class="navbar-brand" :to="{ name: 'search' }"
          >Search</router-link
        >|
        <router-link class="navbar-brand" :to="{ name: 'about' }"
          >About</router-link
        >|
        <span v-if="!$root.store.username">
          <span id="welcome-guest">Hello Guest!</span>

          <router-link class="navbar-brand" :to="{ name: 'register' }"
            >Register</router-link
          >|
          <router-link class="navbar-brand" :to="{ name: 'login' }"
            >Login</router-link
          >|
        </span>
        <span v-else>
          <span id="logged-in-user">
            <span>Hello {{ $root.store.username }}</span>
            <button @click="Logout">Logout</button>|
          </span>
        </span>
      </nav>
    </div>

    <router-view />
  </div>
</template>

<script>
export default {
  name: "App",
  components: {},
  methods: {
    Logout() {
      this.$root.store.logout();
      this.$root.toast("Logout", "User logged out successfully", "success");

      this.$router.push("/").catch(() => {
        this.$forceUpdate();
      });
    },
  },
};
</script>

<style lang="scss">
@import "@/scss/form-style.scss";

#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: black;
  min-height: 100vh;
  background-image: "../resources/b2.jpg";
  background-color: #e5edf0;
}

.navbar-brand {
  margin: 0 10px 0 10px;
}

#logged-in-user,
#welcome-guest {
  position: absolute;
  right: 0px;
  font-size: 1.25rem;
  padding: 0.3125rem 1rem 0.3125rem;
}

#welcome-guest {
  margin-right: 5px;
  color: orange;
}

#logged-in-user span {
  margin-right: 15px;
  font-size: 1.15rem;
}

#logged-in-user button {
  background-color: rgb(239, 221, 186);
  border: 1px solid;
  border-radius: 4px;
}
</style>
