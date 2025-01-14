<template>
  <section class="login-wallpaper">
    <div class="outer-container">
      <div class="login-container">
        <!-- Left Column -->
        <div class="welcome-section">
          <h1 class="welcome-text">Welcome!</h1>
          <p class="welcome-description">
            <b
              >"Even the smallest person can change the course of the
              future."</b
            ><br />
            Dive into the world of The Lord of the Rings with trivia from both
            the books and movies. Share your own trivia or test your knowledge
            on Middle-earth!
          </p>
        </div>

        <div class="login-page card pb-5">
          <h1 class="is-size-3 pt-6 text-black has-text-weight-semibold">
            Sign in
          </h1>
          <div class="container-form">
            <div class="form">
              <!-- Email field -->
              <div class="input-container mt-5">
                <div class="field">
                  <div class="control has-icons-right">
                    <input
                      class="input-transparent"
                      :class="{
                        'is-success': isEmailValid,
                        'is-danger': isEmailInvalid,
                      }"
                      type="text"
                      v-model="email"
                      placeholder="Email"
                      @input="checkForEmailFromat"
                    />

                    <span
                      class="icon is-small is-right"
                      v-if="email && (isEmailValid || isEmailInvalid)"
                    >
                      <i
                        :class="{
                          'fas fa-check': isEmailValid,
                          'fas fa-times': isEmailInvalid,
                        }"
                        :style="{ color: isEmailValid ? '#FFFFFF' : '#950606' }"
                      ></i>
                    </span>
                  </div>

                  <p
                    v-if="isEmailInvalid && !isEmailExists"
                    class="error-message has-text-weight-semibold"
                  >
                    Email does not exist
                  </p>
                </div>
              </div>

              <!-- Password field -->

              <div class="input-container mt-3">
                <div class="field">
                  <div class="control has-icons-right">
                    <input
                      class="input-transparent"
                      :class="{
                        'is-success': isPasswordValid,
                        'is-danger': isPasswordInvalid,
                      }"
                      type="password"
                      v-model="password"
                      placeholder="Password"
                      @input="checkForPasswordError"
                    />

                    <span
                      class="icon is-small is-right"
                      v-if="password && (isPasswordValid || isPasswordInvalid)"
                    >
                      <i
                        :class="{
                          'fas fa-check': isPasswordValid,
                          'fas fa-times': isPasswordInvalid,
                        }"
                        :style="{
                          color: isPasswordValid ? '#FFFFFF' : '#950606',
                        }"
                      ></i>
                    </span>
                  </div>
                  <p
                    v-if="isPasswordInvalid"
                    class="error-message has-text-weight-semibold"
                  >
                    Invalid password
                  </p>
                </div>
              </div>
            </div>

            <div class="center">
              <button
                class="button colors has-text-white"
                :disabled="!isEmailValid || !isPasswordValid"
                v-on:click="login"
              >
                Submit
              </button>
            </div>
          </div>

          <div class="login-link">
            <p class="text-black">
              Don't have an account?
              <router-link to="/sign-up" class="has-text-weight-semibold">
                Sign Up
              </router-link>
            </p>
          </div>
        </div>
      </div>
    </div>
  </section>
</template>

<script>
import axios from "axios";
export default {
  name: "LogIn",
  data() {
    return {
      email: "",
      password: "",
      isEmailValid: false,
      isEmailInvalid: false,
      isPasswordValid: false,
      isPasswordInvalid: false,
      isEmailExists: true, // Initially assume email exists
    };
  },

  methods: {
    async checkForEmailFromat() {
      /* eslint-disable-next-line */
      const emailRegex = /^(([^<>()[\]\\.,;:\s@\"]+(\.[^<>()[\]\\.,;:\s@\"]+)*)|(\".+\"))@(([^<>()[\]\\.,;:\s@\"]+\.)+[^<>()[\]\\.,;:\s@\"]{2,})$/;
      if (emailRegex.test(this.email)) {
        this.isEmailValid = true;
        this.isEmailInvalid = false;
      } else {
        this.isEmailValid = false;
        this.isEmailInvalid = true;
      }

      if (this.email === "") {
        // Reset when email is empty
        this.isEmailValid = false;
        this.isEmailInvalid = false;
        this.isEmailExists = false;
        console.log("error message");
      }
    },

    async checkEmailExists() {
      // Check if email exists in your backend or JSON file
      let result = await axios.get(
        `https://json-server-trivia.onrender.com/users?email=${this.email}`,
      );
      this.isEmailExists = result.data.length > 0;
    },

    checkForPasswordError() {
      // Example rule: password must be at least 6 characters
      if (this.password.length >= 6) {
        this.isPasswordValid = true;
        this.isPasswordInvalid = false;
      } else {
        this.isPasswordValid = false;
        this.isPasswordInvalid = true;
      }

      // Reset states when password field is empty
      if (this.password === "") {
        this.isPasswordValid = false;
        this.isPasswordInvalid = true;
        console.log("error message");
      }
    },

    async login() {
      // First, perform synchronous checks
      this.checkForPasswordError(); // Check password format and length

      // Wait for asynchronous email existence check
      await this.checkEmailExists();
      this.checkForEmailFromat();

      // After both checks are done, validate the login
      if (this.isEmailExists && this.isEmailValid && this.isPasswordValid) {
        // Check if email exists in your data source (JSON file)
        let result = await axios.get(
          `https://json-server-trivia.onrender.com/users?email=${this.email}&password=${this.password}`,
        );
        // If email exists and password matches
        if (result.status === 200 && result.data.length > 0) {
          localStorage.setItem(
            "user-credentials",
            JSON.stringify(result.data[0])
          );
          this.$router.push({ name: "HomePage" });
        }
        // Password does not match
        else {
          this.isPasswordInvalid = true;
        }
        // Email is invalid or does not exist
      } else {
        if (!this.isEmailExists) {
          this.isEmailInvalid = true;
        }
      }
    },
  },
  mounted() {
    let user = localStorage.getItem("user-credentials");
    if (user) {
      this.$router.push({ name: "HomePage" });
    }
  },
};
</script>

<style>
.outer-container {
  margin-top: 50px;
  margin-left: 50px;
  margin-right: 50px;
  margin-bottom: 50px;
  background: transparent;
  box-shadow: 0 0 60px rgba(0, 0, 0, 1); /* Shadow effect */
}

.login-container {
  display: flex;
  justify-content: center;
  align-items: center; /* Reduced space between the sections */
  padding: 60px;
}

.login-page,
.welcome-section {
  flex: 1;
  text-align: center;
  max-width: fit-content;
}

.welcome-section {
  color: white;
  text-align: left;
  max-width: 600px;
  padding-right: 70px;
}

.welcome-text {
  font-size: 2.5rem;
  font-weight: bold;
  margin-bottom: 10px;
}

.welcome-description {
  font-size: 1rem;
  line-height: 1.5;
  margin-bottom: 20px;
}

.text-black {
  color: white;
}

.login-wallpaper {
  background-image: url("../assets/doorsOfMoria.jpeg");
  background-size: cover;
  display: flex;
  justify-content: center;
  align-items: center;
}

.login-page {
  max-width: 400px;
  margin: auto;
  text-align: center;
}

.form {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
}

.login-link p {
  font-size: small;
}

.login-link a {
  text-decoration: none;
  color: #ffffff;
  -moz-transition: all 0.4s ease-in-out;
  -o-transition: all 0.4s ease-in-out;
  -webkit-transition: all 0.4s ease-in-out;
  transition: all 0.4s ease-in-out;
}

.login-link a:hover {
  color: #4390cb;
  transition: 0.25s;
}

.button {
  width: 200px;
  font-size: 16px;
  font-weight: 600;
  color: #fff;
  cursor: pointer;
  margin: 20px;
  height: 55px;
  text-align: center;
  border: none;
  background-size: 300% 100%;
  border-radius: 50px;
  -moz-transition: all 0.4s ease-in-out;
  -o-transition: all 0.4s ease-in-out;
  -webkit-transition: all 0.4s ease-in-out;
  transition: all 0.4s ease-in-out;
}

.button:hover {
  background-position: 100% 0;
  -moz-transition: all 0.4s ease-in-out;
  -o-transition: all 0.4s ease-in-out;
  -webkit-transition: all 0.4s ease-in-out;
  transition: all 0.4s ease-in-out;
}

.button:focus {
  outline: none;
}

.colors {
  background-image: linear-gradient(
    to right,
    #4870dd,
    #4870dd,
    #393bb4,
    #393bb4
  );
}

.card {
  background-color: rgba(135, 135, 135, 0.1);
  border-radius: 25px;
  backdrop-filter: blur(4px);
  border: 2px solid white;
  max-width: 350px;
  /* Ensure the card width matches the login-page */
  border-radius: 15px;
}

.center {
  display: flex;
  justify-content: center;
}

.error-message {
  color: #ff2727;
  font-weight: bold;
  text-align: left;
  font-size: 12px;
}

.input-container {
  /* position: relative; */
  border-bottom: white;
  margin-bottom: 35px;
}

.input-transparent {
  width: 100%;
  padding: 10px 20px 10px 5px;
  background: transparent;
  border-top: none;
  border-left: none;
  border-right: none;
  border-bottom: 2px solid white;
  outline: none;
  font-size: 16px;
  transition: all 0.3s ease;
}

.input-transparent::placeholder {
  color: white;
  font-size: 14px;
  /* padding-bottom: 0px; */
}

.input-transparent:focus,
.input-transparent:valid {
  color: white; /* Set text color to white when focused or valid */
}

@media (min-width: 767px) and (max-width: 852px) {
  .input-transparent {
    border-bottom: 2px solid white;
  }
  .input-transparent::placeholder {
    color: white;
    font-size: 16px;
  }
  .text-black {
    color: white;
  }
  .form {
    padding-left: 20px;
    padding-right: 20px;
  }
}

/* Landscape phones and portrait tablets */
@media (max-width: 767px) {
  .input-transparent {
    border-bottom: 2px solid white;
  }
  .input-transparent::placeholder {
    color: white;
    font-size: 12px;
  }

  .form {
    padding-left: 20px;
    padding-right: 20px;
  }

  .text-black {
    color: white;
  }

  .login-container {
    flex-direction: column;
    align-items: center;
    padding: 20px;
  }

  .welcome-section {
    text-align: center;
    margin-bottom: 50px;
    padding-right: 0px;
  }

  .login-page {
    width: 100%;
  }
}
</style>
