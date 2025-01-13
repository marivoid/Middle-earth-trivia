<template>
  <section class="login-wallpaper">
    <div class="outer-container">
      <div class="login-container">
        <!-- Left Column -->
        <div class="welcome-section">
          <h1 class="welcome-text">Sign Up for Middle-earth Trivia</h1>
          <p class="welcome-description">
            <b
              >"Join the fellowship of fans and share your love for The Lord of
              the Rings!"</b><br/>
            Create your account to contribute your own trivia, explore others'
            insights, and become part of the Middle-earth community!
          </p>
        </div>
        <!-- Right Column -->
        <div class="login-page card pb-5">
          <h1 class="is-size-3 pt-6 text-black has-text-weight-semibold">
            Sign up
          </h1>
          <div class="form">
            <!-- Username field -->
            <div class="input-container mt-4">
              <div class="field">
                <div class="control has-icons-right">
                  <input
                    class="input-transparent"
                    :class="{
                      'is-danger': isUsernameInvalid,
                      'is-success': isUsernameValid,
                    }"
                    type="text"
                    v-model="username"
                    placeholder="Username"
                    @input="checkForUsernameError"
                  />
                  <span v-if="username" class="icon is-small is-right">
                    <i
                      :class="{
                        'fas fa-check': isUsernameValid && !isUsernameTaken,
                        'fas fa-times': isUsernameInvalid || isUsernameTaken,
                      }"
                      :style="{
                        color:
                          isUsernameValid && !isUsernameTaken
                            ? '#00D1B2'
                            : '#FF3860',
                      }"
                    ></i>
                  </span>
                  <p
                    v-if="isUsernameInvalid || isUsernameTaken"
                    class="error-message has-text-weight-semibold"
                  >
                    {{ usernameErrorMessage }}
                  </p>
                </div>
              </div>
            </div>

            <!-- Email field -->
            <div class="input-container mt-2">
              <div class="field">
                <div class="control has-icons-right">
                  <input
                    class="input-transparent"
                    :class="{
                      'is-success': isEmailValid && !isEmailTaken,
                      'is-danger': isEmailInvalid || isEmailTaken,
                    }"
                    type="text"
                    v-model="email"
                    placeholder="Email"
                    @input="checkForEmailFormat"
                  />
                  <span
                    v-if="
                      email && (isEmailValid || isEmailInvalid || isEmailTaken)
                    "
                    class="icon is-small is-right"
                  >
                    <i
                      :class="{
                        'fas fa-check': isEmailValid && !isEmailTaken,
                        'fas fa-times': isEmailInvalid || isEmailTaken,
                      }"
                      :style="{
                        color:
                          isEmailValid && !isEmailTaken ? '#00D1B2' : '#FF3860',
                      }"
                    ></i>
                  </span>
                  <p
                    v-if="isEmailTaken"
                    class="error-message has-text-weight-semibold"
                  >
                    {{ emailErrorMessage }}
                  </p>
                  <p
                    v-if="isEmailInvalid"
                    class="error-message has-text-weight-semibol"
                  >
                    {{ emailErrorMessage }}
                  </p>
                </div>
              </div>
            </div>

            <!-- Password field -->
            <div class="input-container mt-3">
              <div class="field">
                <div class="control has-icons-right">
                  <input
                    class="input-transparent"
                    :class="{
                      'is-danger': isPasswordInvalid,
                      'is-success': isPasswordValid,
                    }"
                    type="password"
                    v-model="password"
                    placeholder="Password"
                    @input="checkForPasswordError"
                  />
                  <span v-if="password" class="icon is-small is-right">
                    <i
                      :class="{
                        'fas fa-check': isPasswordValid,
                        'fas fa-times': isPasswordInvalid,
                      }"
                      :style="{
                        color: isPasswordValid ? '#00D1B2' : '#FF3860',
                      }"
                    ></i>
                  </span>
                  <p
                    v-if="isPasswordInvalid"
                    class="error-message has-text-weight-semibold"
                  >
                    {{ passwordErrorMessage }}
                  </p>
                </div>
              </div>
            </div>

            <!-- Submit Button -->
            <button
              class="button colors mt-6 has-text-white"
              :disabled="
                !isUsernameValid ||
                isUsernameTaken ||
                !isEmailValid ||
                !isPasswordValid
              "
              @click="signUp"
            >
              Sign up
            </button>

            <!-- Login Link -->
            <div class="login-link">
              <p class="text-black">
                Already have an account? <router-link to="/">Login</router-link>
              </p>
            </div>
          </div>
        </div>
      </div>
    </div>
  </section>
</template>

<script>
import axios from "axios";

export default {
  name: "SignUp",
  data() {
    return {
      username: "",
      email: "",
      password: "",
      isUsernameValid: false,
      isUsernameInvalid: false,
      isEmailValid: false,
      isEmailInvalid: false,
      isPasswordValid: false,
      isPasswordInvalid: false,

      usernameErrorMessage: "",
      emailErrorMessage: "",
      passwordErrorMessage: "",

      isUsernameTaken: false,
      isEmailTaken: false,
    };
  },
  methods: {
    async checkForUsernameError() {
      if (!this.username) {
        this.isUsernameValid = false;
        this.isUsernameInvalid = true;
        this.usernameErrorMessage = "Username is required.";
        return;
      }

      // checking if username already exists
      try {
        const response = await axios.get("http://localhost:3000/users", {
          params: { username: this.username },
        });

        if (response.data.length > 0) {
          this.isUsernameValid = false;
          this.isUsernameInvalid = true;
          this.usernameErrorMessage = "Username is already taken.";
          this.isUsernameTaken = true;
        } else {
          this.isUsernameValid = true;
          this.isUsernameInvalid = false;
          this.usernameErrorMessage = "";
          this.isUsernameTaken = false;
        }
      } catch (error) {
        console.error("Error checking username availability:", error);
      }
    },

    checkForEmailFormat() {
      const emailRegex = /^[^\s@]+@[^\s@]+\.[^\s@]+$/;

      if (emailRegex.test(this.email)) {
        this.isEmailValid = true;
        this.isEmailInvalid = false;

        this.checkIfEmailIsTaken();
      } else {
        this.isEmailValid = false;
        this.isEmailInvalid = true;
        this.isEmailTaken = false; // Reset state if format is invalid
      }

      if (this.email === "") {
        this.isEmailValid = false;
        this.isEmailInvalid = false;
        this.isEmailTaken = false; // Reset state when email is empty
      }
    },

    async checkIfEmailIsTaken() {
      try {
        const response = await axios.get(
          `http://localhost:3000/users?email=${this.email}`
        );

        // If the response contains any users, the email is taken
        this.isEmailTaken = response.data.length > 0;
        this.emailErrorMessage = "Email is already taken.";
      } catch (error) {
        console.error("Error checking email:", error);
      }
    },

    checkForPasswordError() {
      if (this.password.length >= 6) {
        this.isPasswordValid = true;
        this.isPasswordInvalid = false;
        this.passwordErrorMessage = "";
      } else {
        this.isPasswordValid = false;
        this.isPasswordInvalid = true;
        this.passwordErrorMessage = "Must be at least 6 characters.";
      }
    },

    async signUp() {
      if (
        !this.isUsernameValid ||
        this.isUsernameTaken ||
        this.isEmailTaken ||
        !this.isEmailValid ||
        !this.isPasswordValid
      ) {
        console.log("Please fix the errors before submitting.");
        return;
      }

      // API call to register the user
      try {
        const response = await axios.post("http://localhost:3000/users", {
          username: this.username,
          email: this.email,
          password: this.password,
        });

        if (response.status === 201) {
          localStorage.setItem(
            "user-credentials",
            JSON.stringify(response.data)
          );
          this.$router.push({ name: "HomePage" });
        }
      } catch (error) {
        console.error("Error signing up:", error);
      }
    },
  },
  mounted() {
    const user = localStorage.getItem("user-credentials");
    if (user) {
      this.$router.push({ name: "HomePage" });
    }
  },
};
</script>
