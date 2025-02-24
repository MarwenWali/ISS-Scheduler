<script setup>
import SchedulerStepper from '@/components/SchedulerStepper.vue'
</script>

<template>
  <header class="bg-blue-600 text-white p-4 shadow-lg">
    <h1 class="text-2xl font-bold">Schedule generator</h1>
  </header>
  <main class="container mx-auto p-4">
    <SchedulerStepper />
  </main>
</template>

<style>
@tailwind base;
@tailwind components;
@tailwind utilities;
</style>