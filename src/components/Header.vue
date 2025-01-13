<template>
  <header class="navbar is-transparent is-spaced black">
    <div class="container">
      <div class="navbar-brand is-family-secondary">
        <!-- @click handler goes here, make sure to scroll a bit to see it. -->
        <a
          :aria-expanded="isActive"
          :class="{ 'is-active': isActive }"
          role="button"
          class="navbar-burger"
          aria-label="menu"
          data-target="collapse"
          @click="isActive = !isActive"
        >
          <span aria-hidden="true" />
          <span aria-hidden="true" />
          <span aria-hidden="true" />
        </a>
      </div>
      <div
        id="collapse"
        :class="{ 'is-active': isActive }"
        class="navbar-menu is-paddingless"
      >
        <nav class="navbar-end black">
          <router-link to="/" class="navbar-item has-text-white"
            >Home</router-link
          >
          <router-link to="/addTrivia" class="navbar-item has-text-white"
            >Add trivia</router-link
          >
          <a v-on:click="logout" href="#" class="navbar-item has-text-white"
            >Logout</a
          >
        </nav>
      </div>
    </div>
  </header>
</template>

<script>
export default {
  data() {
    return {
      // control variables
      isActive: false,
      showNavbar: true,
    };
  },
  // eslint-disable-next-line vue/multi-word-component-names
  name: "Header",
  methods: {
    logout() {
      localStorage.clear();
      this.$router.push({ name: "LogIn" });
    },
  },
};
</script>

<style>
.black {
  background-color: black;
}

@media (min-width: 768px) and (max-width: 1025px) {
  .black {
    background-color: transparent;
  }
}

@media (max-width: 767px) {
  .black {
    background-color: transparent;
  }
}

@media only screen and (min-width: 992px) {
  .black {
    background-color: transparent;
  }
}

/* @media only screen and (min-width: 1200px) {
  .black {
    background-color: transparent;
  }
}  */

a {
  cursor: pointer;
}
</style>
