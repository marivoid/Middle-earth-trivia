<template>
  <section class="bg huge-padding">
    <Header />
    <div class="columns is-vcentered mx-6 mt-6 has-text-white">
      <div class="column is-6 has-text-left mt-6 px-5">
        <h1 class="title mt-4 has-text-white">Add your Middle-earth trivia!</h1>
        <div class="subtitle has-text-white mt-6">
          Share your knowledge of The Lord of the Rings and contribute to the
          growing archive of fascinating facts! Fill out the form to add your
          own trivia piece for others to explore and enjoy. So whatcha waiting
          for? Share them facts!
        </div>
      </div>

      <!-- Form for adding trivia -->
      <div class="column is-6 mt-6 px-5">
        <div>
          <h1
            class="title has-text-left is-size-4 has-text-weight-normal mt-6 has-text-white"
          >
            Give your trivia an intuitive name
          </h1>
          <form class="add">
            <!-- two way binding (v-model) -->
            <input
              class="input"
              type="text"
              name="title"
              placeholder="Enter title"
              v-model="trivia.title"
            />
            <textarea
              class="textarea mt-3"
              type="text"
              name="content"
              placeholder="Enter content"
              v-model="trivia.content"
            ></textarea>
            <button
              class="mt-5 button colors has-text-white mb-6"
              type="button"
              v-on:click="add"
            >
              Add fact
            </button>
          </form>
        </div>
      </div>
    </div>
  </section>

  <!-- Footer -->
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
      <p class="pt-4">Copyright &copy; 2023 Lord of the Rings Trivia</p>
      <p class="has-text-weight-light">Marina Mustac</p>
    </div>
  </footer>
</template>

<script>
import Header from "./Header.vue";
import axios from "axios";

export default {
  name: "AddTrivia",
  components: {
    Header,
  },
  data() {
    return {
      trivia: {
        title: "",
        content: "",
      },
    };
  },
  methods: {
    async add() {
      console.log(this.trivia);
      const result = await axios.post(
        "https://api.jsonbin.io/v3/b/6464a2c6f3a58b46bcfe5f10/trivia",
        {
          headers: {
            "X-Master-Key":
              "$2a$10$OtJPAyDO.N6eLAcMaC1.OO3k0ZABhJlJkX2UoY15NciTkH1XHYPeK",
          },
          title: this.trivia.title,
          content: this.trivia.content,
        }
      );
      if (result.status == 201) {
        // if the new trivia was created, redirect me to the homepage
        this.$router.push({ name: "HomePage" });
      }
    },
  },
  mounted() {
    let user = localStorage.getItem("user-credentials");
    if (!user) {
      this.$router.push({ name: "SignUp" });
    }
  },
};
</script>

<style>
.bg {
  background-image: url(../assets/background.jpg);
  background-size: cover;
  background-position: center center;
}

.huge-padding {
  padding-bottom: 200px;
}

@media (min-width: 768px) and (max-width: 1025px) {
  .is-size-5 {
    font-size: 24px;
  }
}

/* Landscape phones and portrait tablets */
@media (max-width: 767px) {
  .is-size-5 {
    font-size: 18px;
  }
}
</style>
