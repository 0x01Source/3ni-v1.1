<template>
    <div class="mt-32">
    <div class="navbar bg-black bg-opacity-30 px-10 lg:px-80 w-full   mx-auto absolute bottom-0 left-0 right-0  z-50">
      <div class="flex-1">
        <router-link to="/">
          <a class="btn btn-ghost text-xl w-16 h-16 p-0 rounded-full">
            <img src="../../assets/logo.png" alt="Logo" class="w-16" />
          </a>
        </router-link>
      </div>
      <div class="flex-none">
        <div >
          <!-- Actual content -->
          <div  class="dropdown dropdown-end">
            <div>
              <router-link to="/">
                <button class="relative inline-flex items-center justify-center p-0.5 mb-2 me-2 overflow-hidden text-sm font-medium text-gray-900 rounded-lg group bg-gradient-to-br from-green-400 to-blue-600 group-hover:from-green-400 group-hover:to-blue-600 hover:text-white dark:text-white focus:ring-4 focus:outline-none focus:ring-green-200 dark:focus:ring-green-800">
<span class="relative px-5 py-3 transition-all ease-in duration-75 bg-white dark:bg-gray-900 rounded-md group-hover:bg-opacity-0">
    Visit Site

</span>
</button>
            
           
              </router-link>
              <router-link to="/signup">
                <button
                  class="text-white bg-gradient-to-br from-green-400 to-blue-600 border-none hover:bg-gradient-to-bl focus:ring-4 focus:outline-none focus:ring-green-200 font-medium rounded-lg text-sm px-4 py-3 text-center btn">
                  Get Started
                </button>
              </router-link>
            </div>
          </div>

        </div>
      </div>
    </div>
</div>
  </template>
  
  <script>  
  export default {
    name: "FooterprofileComponent",
  };
  </script>
  