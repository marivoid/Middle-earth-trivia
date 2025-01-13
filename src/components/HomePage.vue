<template>
  <section class="hero is-large frodo">
    <Header />
    <div class="hero-body">
      <div class="has-text-left">
        <p class="title aqrada">Middle Earth Trivia</p>
        <p class="subtitle has-text-white mt-6">
          "There is some good in this world, and it’s worth fighting for."
          <br />
          Welcome back {{ username }}!
        </p>
      </div>
    </div>
  </section>

  <div class="spacer"></div>

  <section class="bg-color">
    <div class="container">
      <h1 class="title has-text-white m-6 is-size-1">Trivia</h1>
      <div>
        <div v-for="fact in trivia" :key="fact.id" class="size">
          <h1 class="is-size-3 has-text-white mt-6 has-text-weight-semibold">
            {{ fact.title }}
          </h1>
          <p class="has-text-white mb-6">{{ fact.content }}</p>
        </div>
      </div>
    </div>
  </section>

  <footer class="footer pb-6">
    <div class="columns">
      <div class="column is-4 has-text-centered light">
        <img src="../assets/favicon.png" alt="logo" class="small-logo mt-5" />
        <p class="mt-4 has-text-weight-light">
          Share your most interesting fact with<br />fellow LOTR fans!
        </p>
      </div>

      <div class="column is-4 has-text-centered light">
        <h2 class="mt-6 has-text-weight-semibold">Contact</h2>
        <div class="mt-4 has-text-weight-light">
          <p class="pt-2">lordoftheringstrivia@gmail.com</p>
          <p class="pt-2">+1-585-555-0152</p>
        </div>
      </div>

      <div class="column is-4 has-text-centered light">
        <h2 class="mt-6 has-text-weight-semibold">Our Socials</h2>
        <div class="mt-4 has-text-weight-light">
          <img
            src="../assets/ig2.png"
            alt="instagram logo"
            class="pt-2 socials"
          />
          <img
            src="../assets/tw2.png"
            alt="twitter logo"
            class="px-5 socials"
          />

          <img src="../assets/fb2.png" alt="facebook logo" class="socials" />
        </div>
      </div>
    </div>
    <hr />
    <div class="content has-text-centered light">
      <p class="pt-4">Copyright &copy; 2023 Middle Earth Trivia</p>
      <p class="has-text-weight-light">Marina Mustac</p>
    </div>
  </footer>
</template>

<script>
import Header from "./Header.vue";
import axios from "axios";
export default {
  name: "Home-page",
  data() {
    return {
      username: "",
      trivia: [],
    };
  },
  components: {
    Header,
  },

  async mounted() {
    let user = localStorage.getItem("user-credentials");
    this.username = JSON.parse(user).username;
    if (!user) {
      this.$router.push({ name: "SignUp" });
    }

    let result = await axios.get("http://localhost:3000/trivia");
    console.log(result);
    console.log(this.trivia);
    this.trivia = result.data;

  },
  
};
</script>

<style>
.hero.is-large .hero-body {
  padding: 250px 96px;
  padding-bottom: 360px;
}

.aqrada {
  font-family: "aqrada";
  font-size: 50px;
  letter-spacing: 3px;
  line-height: 100px;
  color: #ffcc24;
}

.bg-color {
  background-color: black;
}

hr {
  background-color: rgb(25, 25, 25);
}

.frodo {
  height: 100vh;
  background-image: url("../assets/frororeading.webp");
  background-size: cover;
  background-position: center center;
  /* background-attachment: fixed; */
  transition: all 0.5s ease;
}

.frodo.scrolled {
  background-color: rgba(0, 0, 0, 0.7); /* Fades into black when scrolled */
  opacity: 0.8; /* Slightly transparent */
}

.light {
  color: rgb(123, 123, 123);
}

.center {
  text-align: center;
}

.column {
  padding: 0;
}

.socials {
  cursor: pointer;
}

.footer {
  background-color: rgb(10, 10, 10);
  padding-top: 75px;
  padding-bottom: 70px;
}

.footer-item {
  margin-left: 48px;
}

.size {
  width: 65%;
  margin-left: 48px;
  margin-right: 48px;
}

@media (min-width: 768px) and (max-width: 1025px) {
  .login-wallpaper {
    background-image: none;
  }

  .hero.is-large .hero-body {
    padding: 150px 96px;
    /* padding-bottom: 30px; */
  }

  .title {
    font-size: 35px;
  }
  .subtitle {
    font-size: 15px;
  }
  /* .mt-6 {
    margin-top: 0px;
  } */
}

/* Landscape phones and portrait tablets */
@media (max-width: 767px) {
  .login-wallpaper {
    background-image: none;
  }

  .hero.is-large .hero-body {
    padding: 50px 96px;
    /* margin-bottom: 35px; */
  }

  .title {
    font-size: 25px;
  }
  .subtitle {
    font-size: 15px;
  }
}
</style>
