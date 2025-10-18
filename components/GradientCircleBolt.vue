<template>
  <div class="relative flex items-center justify-center">
    <svg class="w-max transform -rotate-90">
      <!-- Background circle -->
      <circle
        class="text-gray-200"
        stroke-width="8"
        stroke="currentColor"
        fill="transparent"
        r="45"
        cx="50%"
        cy="50%"
      />

      <!-- Gradient definition -->
      <defs>
        <linearGradient id="gradientFill" x1="0%" y1="0%" x2="100%" y2="100%">
          <stop offset="0%" stop-color="#FF7342" />
          <stop offset="100%" stop-color="#B176FF" />
        </linearGradient>
      </defs>

      <!-- Progress border -->
      <circle
        class="transition-all duration-700 ease-in-out"
        stroke="url(#gradientFill)"
        stroke-width="8"
        stroke-linecap="round"
        fill="transparent"
        r="45"
        cx="50%"
        cy="50%"
        :stroke-dasharray="circumference"
        :stroke-dashoffset="dashOffset"
      />
    </svg>

    <!-- Center Bolt icon -->
    <BoltIcon class="absolute w-10 h-10 text-[#FF7342]" />
  </div>
</template>

<script lang="ts">
import { BoltIcon } from '@heroicons/vue/24/solid'

export default {
  name: 'GradientCircleBolt',
  components: { BoltIcon },
  props: {
    value: {
      type: Number,
      required: true,
      validator: (v: number) => v >= 0 && v <= 100,
    },
  },
  computed: {
    circumference(): number {
      const radius = 45
      return 2 * Math.PI * radius
    },
    dashOffset(): number {
      return this.circumference * (1 - this.value / 100)
    },
  },
}
</script>

<style scoped>
svg circle {
  transition: stroke-dashoffset 0.7s ease;
}
</style>
