<script setup>
defineProps(['data']);

</script>

<template>
 <div class="w-sm h-lg my-4 p-5 overflow-y-auto rounded-t-md border-emerald-600 border-2
  flex flex-wrap gap-1">
    <div v-for="([init, compl], i) in data" :key="i" class="flex-none text-center shrink">
      <span class="text-red-600 block">{{ init }}</span>
      <span class="block font-bold">|</span>
      <span class="text-blue-700 block">{{ compl }}</span>
    </div>
  </div>
</template>
