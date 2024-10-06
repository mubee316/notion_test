<template>
  <div class="bg-gray-200 py-10 flex flex-col items-center">

    <div class="flex flex-col items-center w-full px-4 sm:flex-row sm:justify-center sm:px-0 mb-10">
      <input 
        v-model="search" 
        @input="searchPhotos" 
        class="w-full sm:w-[500px] lg:w-[700px] xl:w-[900px] h-12 px-4 py-2 border border-gray-300 rounded-lg" 
        placeholder="Search for photo"
      />
    </div>

    <SkeletonLoader v-if="loading"  class="bg-white"/>

    <ImageGrid v-else :photos="results" class="bg-white"/>
    <LandingImage  v-if="!search"  :photos="africanPhotos"  class="bg-white"/>
  </div>
</template>

<script setup lang="ts">
import { onMounted, ref } from 'vue';
import axios from 'axios';
import SkeletonLoader from './components/SkeletonLoader.vue'; 
import ImageGrid from './components/ImageGrid.vue'; 
import LandingImage from './components/LandingImage.vue';



const search = ref<string>(''); 
const results = ref<any[]>([]); 
const africanPhotos = ref<any[]>([]);
const loading = ref<boolean>(false); 


const searchPhotos = async () => {
  if (!search.value) {
    results.value = []; 
    loading.value = false;
    return;
  }

  loading.value = true; 

  
  try {
    const response = await axios.get("https://api.unsplash.com/search/photos", {
      params: {
        client_id: 'omsVFLDIllTX2fLvdLufxsHdEahzmow1Lw5ueurUAnM',
        query: search.value,
      },
    });
    results.value = response.data.results;

    await new Promise((resolve) => setTimeout(resolve, 2000)); 


  } catch (error) {
    console.error('Error fetching data:', error);
  } finally {
    loading.value = false; 
  }
};
const fetchAfricanPhotos = async () => {
  try {
    const response = await axios.get("https://api.unsplash.com/search/photos", {
      params: {
        client_id: 'omsVFLDIllTX2fLvdLufxsHdEahzmow1Lw5ueurUAnM',
        query: 'africa',
        per_page: 8,
        order_by: 'latest',
      },
    });
    africanPhotos.value = response.data.results;
    console.log( 'African Photos:', africanPhotos.value);
  } catch (error) {
    console.error('Error fetching data:', error);
  }
};

onMounted(() => {
  fetchAfricanPhotos();
});
</script>


