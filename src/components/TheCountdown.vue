<template>
  <div class="countdown-container">
    <div v-if="countdownFinished">Thank you so much for coming!</div>
    <div v-else class="time-part">
      <div>
        <span class="countdown-part">{{ timeRemaining.days }}</span>
        <p class="countdown-label">day</p>
      </div>
      <div>
        <span class="countdown-part">{{ timeRemaining.hours }}</span>
        <p class="countdown-label">hour</p>
      </div>
      <div>
        <span class="countdown-part">{{ timeRemaining.minutes }}</span>
        <p class="countdown-label">minutes</p>
      </div>
      <div>
        <span class="countdown-part">{{ timeRemaining.seconds }}</span>
        <p class="countdown-label">seconds</p>
      </div>
    </div>
  </div>
</template>
  
  <script setup lang="ts">
  import { ref, computed, onMounted } from 'vue';

  // Set the target date to November 8, 2025
  const targetDate = new Date('2025-11-08T00:00:00Z');
  const currentDate = ref(new Date());
  
  const countdownFinished = computed(() => currentDate.value >= targetDate);
  
  const timeRemaining = computed(() => {
  const totalMilliseconds = Math.floor((targetDate.getTime() - currentDate.value.getTime()));
  const totalSeconds = Math.floor(totalMilliseconds / 1000);

  const days = Math.floor(totalSeconds / (3600 * 24));
  const hours = Math.floor((totalSeconds % (3600 * 24)) / 3600);
  const minutes = Math.floor((totalSeconds % 3600) / 60);
  const seconds = Math.floor(totalSeconds % 60);
  
    return { days, hours, minutes, seconds };
  });
  
  let countdownInterval;
  
  onMounted(() => {
    countdownInterval = setInterval(() => {
      currentDate.value = new Date();
    }, 1000);
  });
  </script>

<style scoped>


.countdown-container {
  display: flex;
  justify-content: center;
  align-items: center;
  font-family: "Mier A", sans-serif !important;
  color: #333;
}

.time-part {
  display: flex;
  gap: 0.75rem;
  align-items: center;
  margin: 0 10px;
}

.countdown-part {
  font-weight: bold;
  font-size: 25px;
  margin-bottom: 5px;
  padding: 5px 10px;
  background-color: #ac4793;
  color: #ffffff;
  border-radius: 5px;
  box-shadow: 0 0 5px rgba(0, 0, 0, 0.3);
}

.countdown-label {
  font-size: 15px;
}
</style> 