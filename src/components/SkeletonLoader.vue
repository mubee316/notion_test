<template>
  <div class="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-3 gap-6 p-4 w-full">
    <div v-for="n in 6" :key="n" class="animate-pulse">
      <div class="bg-gray-300 h-48 rounded-lg"></div>
      <div class="mt-4 space-y-2">
        <div class="h-4 bg-gray-300 rounded w-3/4"></div>
        <div class="h-4 bg-gray-300 rounded w-1/2"></div>
      </div>
    </div>
  </div>
</template>

