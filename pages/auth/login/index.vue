<template>
  <section style="margin-top: 200px">
    <div class="container-fluid h-custom">
      <div class="row d-flex justify-content-center align-items-center h-100">
        <div class="col-md-0 col-lg-3 col-xl-3"></div>
        <div class="col-md-8 col-lg-6 col-xl-6">
          <form @submit.prevent="onSubmit">
            <!-- Email input -->
            <div class="form-outline mb-4">
              <input
                type="text"
                id="form3Example3"
                class="form-control form-control-lg"
                placeholder="Nif"
                v-model="form.nif"
              />
              <label class="form-label" for="form3Example3">NIF</label>
            </div>

            <!-- Password input -->
            <div class="form-outline mb-3">
              <input
                type="password"
                id="form3Example4"
                class="form-control form-control-lg"
                placeholder="Password"
                v-model="form.password"
              />
              <label class="form-label" for="form3Example4">Password</label>
            </div>

            <div class="text-center text-lg-start mt-4 pt-2">
              <v-btn
                type="submit"
                class="btn btn-primary btn-lg"
                style="padding-left: 2.5rem; padding-right: 2.5rem"
              >
                Login
              </v-btn>
              <p v-if="form.error" style="color: red">{{ form.error }}</p>
            </div>
          </form>
        </div>
        <div class="col-md-0 col-lg-3 col-xl-3"></div>
      </div>
    </div>
  </section>
</template>

<script>
export default {
  auth: false,
  data() {
    return {
      form: {
        nif: "",
        password: "",
        error: ""
      },
      show: true,
    };
  },
  methods: {
    onSubmit() {
      let promise = this.$auth.loginWith('local', {
        data: {
          nif: this.form.nif,
          password: this.form.password
        }
      })
      promise.then(() => {
        this.$router.push(`/auth/${this.$auth.user.role.toLowerCase()}`);
      })
      promise.catch(() => {
        this.form.error = 'Invalid nif or password';
      })
    }
  },
};
</script>
<style>
.login-card,
.divider:after,
.divider:before {
  content: "";
  flex: 1;
  height: 1px;
  background: #eee;
}
.h-custom {
  height: calc(100% - 73px);
}
@media (max-width: 450px) {
  .h-custom {
    height: 100%;
  }
}
</style>
