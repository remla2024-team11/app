<template>
  <div class="flex justify-center items-center h-screen">
    <form @submit.prevent="submitForm" class="bg-white shadow-md rounded px-8 pt-6 pb-8 mb-4">
      <div class="flex p-2">
        You are using version {{ version }}
      </div>
      <div class="mb-4">
        <label class="block text-gray-700 text-sm font-bold mb-2" for="inputField">
          Input Field
        </label>
        <input v-model="inputValue" class="shadow appearance-none border rounded w-full py-2 px-3 text-gray-700 leading-tight focus:outline-none focus:shadow-outline" id="inputField" type="text" placeholder="Enter your input...">
      </div>
      <div class="flex items-center justify-between">
        <button class="bg-blue-500 hover:bg-blue-700 text-white font-bold py-2 px-4 rounded focus:outline-none focus:shadow-outline" type="submit">
          Submit
        </button>
        <!-- Loader -->
        <div v-if="isLoading" class="animate-spin rounded-full h-8 w-8 border-t-2 border-b-2 border-blue-500"></div>
      </div>
      <!-- Result Field -->
      <div v-if="name" class="mt-4">
        <p class="text-green-500 font-bold">Result:</p>
        <p>{{ name }}</p>
      </div>
    </form>
  </div>
</template>

<script>
import axios from 'axios';

export default {
  data() {
    return {
      inputValue: '',
      isLoading: false, // Add loading state
      name: null, // Add result field
      age: null,
      salary: null,
      version: null,
    };
  },
  methods: {
    async submitForm() {
      try {
        this.isLoading = true; // Set loading state to true

        // Make the API call
        const response = await axios.post(`${import.meta.env.VITE_API}/post`, {
          title: this.inputValue,
          body: 'bar',
          userId: 1,
        });

        // Handle the response
        console.log('API response:', response.data);
        this.name = response.data.title; 

      } catch (error) {
        // Handle any errors
        console.error('Error making API call:', error);
      } finally {
        this.isLoading = false; // Reset loading state whether the call succeeds or fails
      }
    }
  },
  async mounted() {
    try{
    const response = await axios.get(`${import.meta.env.VITE_API}/version`);
    this.version = response.data.version;}
    catch(error){
      console.error('Error fetching version:', error);
      this.version='0.0.0';
    }
  }
};
</script>

<style>
/* Any additional styling goes here */
</style>
