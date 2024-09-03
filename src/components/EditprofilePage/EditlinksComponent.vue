<template>
  <div class="bg-black">
    <div class="animate-in">
      <div v-if="!hasAccess">
        <div class="bg-black h-screen text-red-600 pt-40 text-bold text-center mx-auto text-2xl font-bold">
          <div><i class="fa-solid fa-user-xmark mb-5 text-8xl"></i></div>
          Sorry .. you don't have access to this page.
        </div>
      </div>
      <div v-else>
        <div class="mb-10">
          <h1 class="text-[#dfa027] mb-2 text-2xl">Edit Links</h1>

          <hr class="w-60 h-1 mb-4 bg-[#dfa027] border-0 rounded" />
        </div>
        <div v-if="profile && profile.links">

          <div v-for="(link, index) in profile.links" :key="link.id" class="mb-4 lg:grid lg:grid-cols-2">

            <details class="collapse  col-span-2 bg-white bg-opacity-5 text-white">
  <summary class="collapse-title text-xl font-medium">{{ link.title }}   <i class="fa-solid fa-circle-chevron-down absolute top-5 right-4"></i> <button @click="removeLink(index, link.id)" class="    text-white hover:text-red-600  px-2 absolute  right-12  rounded-lg">
              <i class="fa-solid fa-trash"></i>
            </button></summary>

  <div class="collapse-content mb-4 ">
    <div class="lg:flex">
    <div class="relative lg:w-1/2 lg:mr-1 mb-2">
              <input type="text" v-model="link.title"
                class="block px-4 py-4 pt-4 w-full text-sm text-white bg-transparent rounded-xl border-1 border-gray-500 appearance-none focus:outline-none focus:ring-0 focus:border-[#dfa027] peer "
                placeholder=" " />
              <label
                class="absolute text-sm text-gray-500 duration-300 transform -translate-y-4 scale-75 top-2 z-10 origin- bg-black px-2 peer-focus:px-4 peer-focus:text-[#dfa027] peer-placeholder-shown:scale-100 peer-placeholder-shown:-translate-y-1/2 peer-placeholder-shown:top-1/2 peer-focus:top-2 peer-focus:scale-75 peer-focus:-translate-y-4 rtl:peer-focus:translate-x-1/4 rtl:peer-focus:left-auto start-1">
                 Title
              </label>
            </div>

            <div class="relative lg:w-1/2 lg:ml-1 mb-2">
            <input type="file" @change="handleIconFileChange($event, index)" class="block px-4 py-1  w-full text-sm text-white bg-transparent rounded-xl border-2  border-gray-500 appearance-none focus:outline-none focus:ring-0 focus:border-[#dfa027] peer "
            />
            <label
                class="absolute text-sm text-gray-500 duration-300 transform -translate-y-4 scale-75 top-2 z-10 origin- bg-black px-2 peer-focus:px-4 peer-focus:text-[#dfa027] peer-placeholder-shown:scale-100 peer-placeholder-shown:-translate-y-1/2 peer-placeholder-shown:top-1/2 peer-focus:top-2 peer-focus:scale-75 peer-focus:-translate-y-4 rtl:peer-focus:translate-x-1/4 rtl:peer-focus:left-auto start-1">
                 Icon <span v-if="link.icon" class="text-[#dfa027]"> - Already Added </span>
              </label>
          </div>
        </div>

        <div class="lg:flex">
<div class="relative  w-28 lg:mr-1 mb-2">
          <button @click="toggleDiv" class="px-4 py-3 my-1 bg-[#dfa027] text-white text-sm rounded-lg">
  URL-File
</button>
</div>

<div v-if="showUrlDiv" class="relative w-full lg:ml-1 mb-2">
  <input type="text" v-model="link.url"
    class="block px-4 py-4 pt-4 w-full text-sm text-white bg-transparent rounded-xl border-1 border-gray-500 appearance-none focus:outline-none focus:ring-0 focus:border-[#dfa027] peer "
    placeholder=" " />
  <label
    class="absolute text-sm text-gray-500 duration-300 transform -translate-y-4 scale-75 top-2 z-10 origin- bg-black px-2 peer-focus:px-4 peer-focus:text-[#dfa027] peer-placeholder-shown:scale-100 peer-placeholder-shown:-translate-y-1/2 peer-placeholder-shown:top-1/2 peer-focus:top-2 peer-focus:scale-75 peer-focus:-translate-y-4 rtl:peer-focus:translate-x-1/4 rtl:peer-focus:left-auto start-1">
    URL
  </label>
</div>

<div v-else class="relative w-full lg:ml-1 mb-2">
  <input type="file" @change="handleFileChange($event, index)" class="block px-4 py-1  w-full text-sm text-white bg-transparent rounded-xl border-2 border-gray-500 appearance-none focus:outline-none focus:ring-0 focus:border-[#dfa027] peer"
  />
  <label
    class="absolute text-sm text-gray-500 duration-300 transform -translate-y-4 scale-75 top-2 z-10 origin- bg-black px-2 peer-focus:px-4 peer-focus:text-[#dfa027] peer-placeholder-shown:scale-100 peer-placeholder-shown:-translate-y-1/2 peer-placeholder-shown:top-1/2 peer-focus:top-2 peer-focus:scale-75 peer-focus:-translate-y-4 rtl:peer-focus:translate-x-1/4 rtl:peer-focus:left-auto start-1">
    File <span v-if="link.file" class="text-[#dfa027]"> - Already Added </span>
  </label>
</div>

</div>

        </div>
      </details>
          
            
            
           

         
          </div>
          <button @click="addLink()" class="btn bg-black hover:bg-[#dfa027] hover:bg-opacity-80 text-white border-gray-600 border-2 hover:border-none py-3 px-6 rounded-lg mt-4 mb-12 mr-2">
            Add Link
          </button>
          <button @click="updateLinks()" class="btn bg-[#dfa027] hover:bg-[#dfa027] hover:bg-opacity-80 text-white border-none py-3 px-6 rounded-lg mt-4 mb-12">
            Save Changes
          </button>
        </div>
        <!-- Loading Indicator -->
        <div v-if="isLoading" class="absolute top-0 left-0 w-full h-full flex items-center justify-center text-[#dfa027] bg-black bg-opacity-50">
          <span class="loading loading-dots loading-lg"></span>
        </div>

        <!-- Toast for Errors and Success -->
        <div class="toast toast-top toast-center mt-20 z-50">
          <div v-if="showErrorToast" id="toast-danger" class="alert alert-info bg-red-700 text-white text-center">
            <span><i class="fa-solid fa-circle-xmark text-white mr-2 fa-2xl"></i>{{ errorMessage }}</span>
          </div>
          <div v-if="showToast" id="toast-success" class="alert alert-success bg-green-700 text-white text-center">
            <span><i class="fa-solid fa-circle-check text-white mr-2 fa-2xl"></i>Update successfully</span>
          </div>
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
      profile: { links: [] },
      isLoading: false,
      showToast: false,
      showErrorToast: false,
      errorMessage: "",
      username: "",
      hasAccess: true,
      linksToDelete: [],
      showUrlDiv: true
    };
  },
  created() {
    this.checkAccess();
  },
  methods: {
    toggleDiv() {
      this.showUrlDiv = !this.showUrlDiv; // Toggle the boolean value
    },
    checkAccess() {
      this.isLoading = true;
      axios
        .get('/auth/users/me/', {
          headers: {
            Authorization: `Bearer ${localStorage.getItem('token')}`,
          },
        })
        .then((response) => {
          this.username = response.data.username;
          const urlUsername = this.$route.params.username;
          if (this.username !== urlUsername) {
            this.hasAccess = false;
          } else {
            this.fetchProfile();
          }
        })
        .catch((error) => {
          console.error('Error fetching username:', error);
          this.isLoading = false;
        });
    },
    fetchProfile() {
      axios
        .get(`/api/profiles/${this.username}/`)
        .then((response) => {
          this.profile = response.data;
        })
        .catch((error) => {
          console.error('Error fetching profile data:', error);
        })
        .finally(() => {
          this.isLoading = false;
        });
    },
    addLink() {
      this.profile.links.push({
        title: "",
        url: "",
        icon: null,
        file: null,
      });
    },
    removeLink(index, id) {
      const link = this.profile.links[index];
      if (!this.linksToDelete) {
        this.linksToDelete = [];
      }
      this.linksToDelete.push(link);
      this.profile.links.splice(index, 1);
    },
    handleIconFileChange(event, index) {
      this.profile.links[index].icon = event.target.files[0];
    },
    handleFileChange(event, index) {
      this.profile.links[index].file = event.target.files[0];
    },
    updateLinks() {
      const token = localStorage.getItem('token');
      this.isLoading = true;
      if (token) {
        const headers = { Authorization: `Bearer ${token}` };
        const requests = [];

        // Handle deletion of links
        if (this.linksToDelete.length) {
          this.linksToDelete.forEach((link) => {
            if (link.id) {
              const request = axios.delete(`http://197.134.253.234:9100/api/links/${link.id}/`, { headers });
              requests.push(request);
            }
          });
          this.linksToDelete = [];
        }

        // Handle updates and additions of links
        this.profile.links.forEach((link) => {
          let request;
          const formData = new FormData();
          formData.append('title', link.title);
          formData.append('url', link.url);
          if (link.icon) formData.append('icon', link.icon);
          if (link.file) formData.append('file', link.file);

          if (link.id) {
            // Existing link - perform update
            request = axios.put(`http://197.134.253.234:9100/api/links/${link.id}/`, formData, { headers });
          } else {
            // New link - perform addition
            request = axios.post('http://197.134.253.234:9100/api/links/', formData, { headers });
          }
          requests.push(request);
        });

        // Execute all requests
        Promise.all(requests)
          .then(() => {
            this.showToast = true;
            setTimeout(() => {
              this.showToast = false;
              window.location.reload();
            }, 2000);
          })
          .catch((error) => {
            this.showErrorToast = true;
            this.errorMessage = 'Failed to update links. Please try again.';
            console.error('Error updating links:', error);
            setTimeout(() => {
              this.showErrorToast = false;
            }, 2000);
          })
          .finally(() => {
            this.isLoading = false;
          });
      }
    },
  },
};
</script>
