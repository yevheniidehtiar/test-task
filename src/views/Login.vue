<template>
  <div class="login">
    <h1 class="mb-2">Login :) </h1>
    <v-form ref="login-form" @submit.prevent="onLoginSubmit">
      <v-text-field
        v-model="loginForm.username"
        :rules="[rules.required]"
        label="Username"
        outlined
      />

      <v-text-field
        v-model="loginForm.password"
        :rules="[rules.required]"
        type="password"
        label="Password"
        outlined
      />

      <v-btn
        color="primary"
        type="submit"
        :loading="loading.submitLogin"
      >
        Login
      </v-btn>
    </v-form>

    <nav class="mt-4">
      <router-link :to="{name: 'PrivacyPolicy'}">Privaci Policy</router-link>
    </nav>
  </div>
</template>

<script>

import { loginRequest } from '@/api/services.js';
import { rules } from '@/common/validations.js'
import { login } from '@/common/auth.js'

export default {
  name: 'Login',
  data() {
    return {
      rules,
      loginForm: {
        username: '',
        password: '',
      },
      loading: {
        submitLogin: false,
      }
    };
  },
  methods: {
    async onLoginSubmit() {
      if (!this.$refs['login-form'].validate()) return;
      this.loading.submitLogin = true;

      try {
        await loginRequest(this.loginForm.username, this.loginForm.password);
        login();
        this.$router.push({ name: 'Crm' });
      } catch (err) {
        this.$notify.error(err.error)
      } finally {
        this.loading.submitLogin = false
      }
    }
  }
}
</script>

<style lang="scss" scoped>
.login {
  width: 300px;
}
</style>