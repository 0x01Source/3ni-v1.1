<template>
  <div>
    <h1>Reset Password</h1>
    <form @submit.prevent="handleReset">
      <input type="password" v-model="password" placeholder="New Password" />
      <input type="password" v-model="passwordConfirm" placeholder="Confirm New Password" />
      <button type="submit">Reset Password</button>
    </form>
  </div>
</template>

<script>
export default {
  data() {
    return {
      password: '',
      passwordConfirm: '',
      uidb64: this.$route.params.uidb64,
      token: this.$route.params.token,
    }
  },
  methods: {
    async handleReset() {
      if (this.password !== this.passwordConfirm) {
        alert('Passwords do not match');
        return;
      }
      try {
        const response = await axios.post('http://197.134.253.234:9100/password/reset/confirm/', {
          uidb64: this.uidb64,
          token: this.token,
          new_password1: this.password,
          new_password2: this.passwordConfirm,
        });
        console.log(response);
        // Redirect to login page after successful reset
        this.$router.push({ name: 'login' });
      } catch (error) {
        console.error(error);
      }
    }
  }
}
</script>