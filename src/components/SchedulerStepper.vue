<script setup>
import { ref } from 'vue'

const steps = ['Courses', 'Options', 'Schedules']
const currentStep = ref(0)
const courses = ref([
  { name: 'INF1007', obligatory: true },
  { name: 'MTH1101', obligatory: true }
])
const optimizationOptions = ref({
  minimizeGaps: true,
  maximizeSleep: false,
  maximizeDaysOff: false,
  minimizeCourses: false,
  minimizeCertificationPeriods: false
})
const newCourse = ref('')
const generatedSchedules = ref([])

const addCourse = () => {
  if (newCourse.value.trim()) {
    courses.value.push({ name: newCourse.value.trim(), obligatory: true })
    newCourse.value = ''
  }
}

const generateSchedules = () => {
  // Mock data for demonstration
  generatedSchedules.value = [
    { 
      courses: ['INF1007', 'MTH1101'],
      days: ['Mon 08:00-12:00', 'Fri 09:00-11:00'],
      gaps: 1
    }
  ]
  currentStep.value = 2
}
</script>

<template>
  <div class="max-w-4xl mx-auto p-6">
    <!-- Stepper -->
    <div class="flex justify-between mb-8">
      <div v-for="(step, index) in steps" :key="step" 
           class="flex flex-col items-center">
        <div :class="[
          'w-8 h-8 rounded-full flex items-center justify-center',
          currentStep >= index ? 'bg-blue-500 text-white' : 'bg-gray-200'
        ]">
          {{ index + 1 }}
        </div>
        <span class="mt-2 text-sm" :class="currentStep >= index ? 'text-blue-500' : 'text-gray-400'">
          {{ step }}
        </span>
      </div>
    </div>

    <!-- Step 1: Course Selection -->
    <div v-show="currentStep === 0" class="space-y-6">
      <h2 class="text-2xl font-bold">Courses in demand</h2>
      <div class="space-y-4">
        <div v-for="(course, index) in courses" :key="index" class="flex items-center gap-4">
          <input v-model="course.name" class="p-2 border rounded w-48">
          <label class="flex items-center gap-2">
            <input type="checkbox" v-model="course.obligatory" class="w-4 h-4">
            <span class="text-sm">Obligatory</span>
          </label>
        </div>
        <div class="flex gap-4">
          <input v-model="newCourse" @keyup.enter="addCourse" 
                 placeholder="New Course" class="p-2 border rounded w-48">
          <button @click="addCourse" class="px-4 py-2 bg-blue-500 text-white rounded hover:bg-blue-600">
            Add
          </button>
        </div>
      </div>
    </div>

    <!-- Step 2: Optimization Options -->
    <div v-show="currentStep === 1" class="space-y-6">
      <h2 class="text-2xl font-bold">Optimization options</h2>
      <div class="grid grid-cols-2 gap-4">
        <label v-for="(value, option) in optimizationOptions" :key="option" 
               class="flex items-center gap-2 p-4 border rounded hover:bg-gray-50">
          <input type="checkbox" v-model="optimizationOptions[option]" class="w-4 h-4">
          <span class="capitalize">{{ option }}</span>
        </label>
      </div>
    </div>

    <!-- Step 3: Generated Schedules -->
    <div v-show="currentStep === 2" class="space-y-6">
      <h2 class="text-2xl font-bold">Generated schedules</h2>
      <div v-for="(schedule, index) in generatedSchedules" :key="index" 
           class="p-6 border rounded bg-gray-50">
        <div class="grid grid-cols-2 gap-4">
          <div>
            <h3 class="font-bold mb-2">Course:</h3>
            <ul class="list-disc pl-4">
              <li v-for="course in schedule.courses" :key="course">{{ course }}</li>
            </ul>
          </div>
          <div>
            <h3 class="font-bold mb-2">Days:</h3>
            <ul class="list-disc pl-4">
              <li v-for="day in schedule.days" :key="day">{{ day }}</li>
            </ul>
          </div>
        </div>
      </div>
    </div>

    <!-- Navigation -->
    <div class="mt-8 flex justify-between">
      <button v-if="currentStep > 0" @click="currentStep--"
              class="px-4 py-2 bg-gray-200 rounded hover:bg-gray-300">
         Return
      </button>
      <button v-else class="invisible"></button>
      
      <button v-if="currentStep < 2" @click="currentStep++"
              class="px-4 py-2 bg-blue-500 text-white rounded hover:bg-blue-600">
        Continue
      </button>
      <button v-else @click="currentStep = 0"
              class="px-4 py-2 bg-blue-500 text-white rounded hover:bg-blue-600">
        Generate

      </button>
    </div>
  </div>
</template>