<template>
  <div class="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-3 gap-6 p-4 w-full">
    <div 
      v-for="photo in photos" 
      :key="photo.id" 
      class="relative cursor-pointer"
      :class="getRandomSize()"
      @click="openDialog(photo)"
    >
      <v-img :src="photo.urls.regular" :alt="photo.alt_description || 'Image by ' + photo.user.name" class="w-full h-full object-cover rounded-lg"></v-img>
      <div class="absolute bottom-0 left-0 right-0 bg-black bg-opacity-50 p-4 text-white rounded-b-lg">
        <h3 class="text-lg font-bold">{{ photo.user.name }}</h3>
        <p class="text-sm">{{ photo.user.location || 'Unknown location' }}</p>
      </div>
    </div>

    <!-- Dialog Component -->
    <v-dialog v-model="dialog" max-width="600px">
      <v-card v-if="selectedPhoto">
        <v-img :src="selectedPhoto.urls.regular" :alt="selectedPhoto.alt_description || 'Image by ' + selectedPhoto.user.name" class="w-full h-full object-cover rounded-lg"></v-img>
        <v-card-title>{{ selectedPhoto.user.name }}</v-card-title>
        <v-card-subtitle>{{ selectedPhoto.user.location || 'Unknown location' }}</v-card-subtitle>
      </v-card>
    </v-dialog>
  </div>
</template>

<script setup lang="ts">
import { ref, defineProps, PropType } from 'vue';

const props = defineProps({
  photos: { 
    type: Array as PropType<any[]>,
    required: true,
  }
});

const dialog = ref(false);
const selectedPhoto = ref(null);

const openDialog = (photo: any) => {
  selectedPhoto.value = photo;
  dialog.value = true;
};

const getRandomSize = (): string => {
  const sizes = ['row-span-2', 'row-span-1'];
  return sizes[Math.floor(Math.random() * sizes.length)];
};
</script>
