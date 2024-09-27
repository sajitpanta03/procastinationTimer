<template>
  <div class="flex flex-col items-center justify-center min-h-screen bg-gray-100 bg-no-repeat bg-cover" style="background-image: url('./public/wallpaper.jpg');">
    <div class="max-w-lg w-full p-6 bg-white shadow-lg rounded-lg">
      <h2 class="text-3xl font-semibold text-center text-gray-800 mb-4">{{ title }}</h2>

      <div class="text-xl text-center text-gray-600 mb-6">{{ formattedTime }}</div>

      <div class="grid grid-cols-1 gap-4">
        <button
            class="p-3 bg-red-600 hover:bg-red-700 text-white rounded-lg font-bold transition-all"
            @click="startTimer"
            :disabled="isRunning">
          Start Slacking
        </button>

        <button
            class="p-3 bg-sky-500 hover:bg-sky-600 text-white rounded-lg font-bold transition-all"
            @click="pauseTimer"
            :disabled="!isRunning">
          Pause (As If You Need It)
        </button>

        <button
            class="p-3 bg-gray-800 hover:bg-gray-900 text-white rounded-lg font-bold transition-all"
            @click="resetTimer">
          Back to "Work"
        </button>
      </div>

      <div v-if="currentAchievement" class="mt-6 p-4 bg-green-100 text-green-700 text-center rounded-lg shadow-sm">
        <p class="font-semibold">Achievement Unlocked: {{ currentAchievement }}</p>
      </div>
    </div>
  </div>
</template>


<script setup>
  import { ref, computed, watch } from 'vue'


      const title = ref('Procrastination Timer: Because Productivity is Overrated')
      const time = ref(0)
      const isRunning = ref(false)
      const interval = ref(null)
      const currentAchievement = ref('')
  
      const achievements = [
        { time: 300, message: "Coffee Break Master" },
        { time: 900, message: "Social Media Guru" },
        { time: 1800, message: "Netflix Episode Completionist" },
        { time: 3600, message: "Professional Time Waster" }
      ]
  
      const formattedTime = computed(() => {
        const hours = Math.floor(time.value / 3600)
        const minutes = Math.floor((time.value % 3600) / 60)
        const seconds = time.value % 60
        return `${hours.toString().padStart(2, '0')}:${minutes
          .toString()
          .padStart(2, '0')}:${seconds.toString().padStart(2, '0')}`
      })
  
      const startTimer = () => {
        if (!isRunning.value) {
          isRunning.value = true
          interval.value = setInterval(() => {
            time.value++
          }, 1000)
        }
      }
  
      const pauseTimer = () => {
        clearInterval(interval.value)
        isRunning.value = false
      }
  
      const resetTimer = () => {
        pauseTimer()
        time.value = 0
        currentAchievement.value = ''
      }
  
      watch(time, (newValue) => {
        const achievement = achievements.find(a => a.time === newValue)
        if (achievement) {
          currentAchievement.value = achievement.message
        }
      })
  </script>
