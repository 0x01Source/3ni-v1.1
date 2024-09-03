<template>
    <div class="flex items-center justify-center bg-black text-white">
      <div class="max-w-md w-full p-6 rounded-lg shadow-md">
        <h2 class="text-2xl font-semibold mb-4 text-center">Reset Password</h2>
        <form @submit.prevent="handleSubmit">
          <div class="mb-4">
            <div class="relative mb-4">
              <input
                type="email"
                v-model="email"
                id="email"
                required
                class="block px-4 py-4 pt-4 w-full text-sm text-white bg-transparent rounded-xl border border-gray-500 appearance-none focus:outline-none focus:ring-0 focus:border-[#dfa027] peer"
                placeholder=" "
              />
              <label
                for="email"
                class="absolute text-sm ml-1 text-gray-500 duration-300 transform -translate-y-4 scale-75 top-2 z-10 bg-black px-2 peer-focus:px-4 peer-focus:text-[#dfa027] peer-placeholder-shown:scale-100 peer-placeholder-shown:-translate-y-1/2 peer-placeholder-shown:top-1/2 peer-focus:top-2 peer-focus:scale-75 peer-focus:-translate-y-4"
              >
                Email
              </label>
            </div>
          </div>
          <button
            type="submit"
            class="w-full text-white bg-gradient-to-r from-yellow-200 via-[#dfa027] to-[#dfa027] hover:bg-gradient-to-bl hover:opacity-80 focus:ring-4 focus:outline-none focus:ring-red-100 font-bold rounded-xl text-lg px-8 py-3 text-center mb-2"
          >
            Send Reset Link
          </button>
        </form>
  
        <!-- Loading Indicator -->
        <div v-if="isLoading" class="absolute top-0 left-0 w-full h-full flex items-center justify-center text-[#dfa027] bg-black bg-opacity-50">
          <span class="loading loading-dots loading-lg"></span>
        </div>
  
        <!-- Toast for Errors and Success -->
        <div class="fixed top-5 left-1/2 transform -translate-x-1/2 z-50 w-full max-w-md">
          <div v-if="errorMessage" class="bg-red-700 text-white text-center p-4 rounded-md mb-2 shadow-lg">
            <i class="fa-solid fa-circle-xmark text-white mr-2"></i>{{ errorMessage }}
          </div>
          <div v-if="successMessage" class="bg-green-700 text-white text-center p-4 rounded-md mb-2 shadow-lg">
            <i class="fa-solid fa-circle-check text-white mr-2"></i>{{ successMessage }}
          </div>
        </div>
      </div>
    </div>
  </template>
  
  <script>
  import axios from 'axios';
  
  export default {
    data() {
      return {
        email: '',
        isLoading: false,
        errorMessage: '',
        successMessage: '',
      };
    },
    methods: {
      async handleSubmit() {
        this.isLoading = true;
        this.errorMessage = '';
        this.successMessage = '';
  
        try {
          const response = await axios.post('auth/users/reset_password/', { email: this.email });
          this.successMessage = 'Check your Email for Reset Link!';
          this.autoHideToast('success');
        } catch (error) {
          this.errorMessage = error.response.data.message || 'An error occurred while sending the reset link.';
          this.autoHideToast('error');
        } finally {
          this.isLoading = false;
        }
      },
      autoHideToast(type) {
        setTimeout(() => {
          if (type === 'success') {
            this.successMessage = '';
          } else if (type === 'error') {
            this.errorMessage = '';
          }
        }, 3000); // Hide after 3 seconds
      }
    },
  };
  </script>
  