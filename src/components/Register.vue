<template>
  <div class="col-md-12">
    <div class="card card-container">
      <img
        id="profile-img"
        src="//ssl.gstatic.com/accounts/ui/avatar_2x.png"
        class="profile-img-card"
      />
      <form @submit="handleRegister">
        <div v-if="!successful">
          <div class="form-group">
            <label for="username">Username</label>
            <ValidationProvider rules="required|alpha|max:10" v-slot="{errors}">
            <input name="username" v-model="username" type="text" class="form-control" />
            <span>{{ errors[0] }}</span>
            </ValidationProvider>
          </div>
          <div class="form-group">
            <label for="email">Email</label>
            <ValidationProvider rules="required|email" v-slot="{errors}">
            <input name="email" v-model="email" type="email" class="form-control" />
            <span>{{ errors[0] }}</span>
            </ValidationProvider>
          </div>
          <div class="form-group">
            <label for="password">Password</label>
            <ValidationProvider rules="required" v-slot="{errors}">
            <input name="password" v-model="password" type="password" class="form-control" />
            <span>{{ errors[0] }}</span>
            </ValidationProvider>
          </div>

          <div class="form-group">
            <button class="btn btn-primary btn-block" :disabled="loading">
              <span
                v-show="loading"
                class="spinner-border spinner-border-sm"
              ></span>
              Sign Up
            </button>
          </div>
        </div>
      </form>

      <div
        v-if="message"
        class="alert"
        :class="successful ? 'alert-success' : 'alert-danger'"
      >
        {{ message }}
      </div>
    </div>
  </div>
</template>

<script>
import { ValidationProvider } from "vee-validate";
import { extend } from 'vee-validate';
import { required, email, max, alpha} from 'vee-validate/dist/rules';

extend('required', required);
extend('email', email);
extend('max', max);
extend('alpha', alpha);

// Object.keys(rules).forEach(rule => {
//   // eslint-disable-next-line
//   extend(rule, ...rules[rule]);
// });

// for (let [rule, validation] of Object.entries(rules)) {
//   // eslint-disable-next-line
//   extend(rule, {
//     ...validation
//   })
// }

export default {
  name: "Register",
  components: {
    ValidationProvider
  },
  data() {
    return {
      successful: false,
      loading: false,
      message: "",
      username: "",
      email: "",
      password: "",

    };
  },
  computed: {
    loggedIn() {
      return this.$store.state.auth.status.loggedIn;
    },
  },
  mounted() {
    if (this.loggedIn) {
      this.$router.push("/profile");
    }
  },
  methods: {
    handleRegister(user) {
      this.message = "";
      this.successful = false;
      this.loading = true;

      this.$store.dispatch("auth/register", user).then(
        (data) => {
          this.message = data.message;
          this.successful = true;
          this.loading = false;
        },
        (error) => {
          this.message =
            (error.response &&
              error.response.data &&
              error.response.data.message) ||
            error.message ||
            error.toString();
          this.successful = false;
          this.loading = false;
        }
      );
    },
  },
};
</script>
