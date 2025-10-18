<template>
  <div v-if="isOpen" class="fixed inset-0 flex items-center justify-center bg-black/50 z-50">
    <div class="bg-white rounded-xl shadow-lg p-6 w-full max-w-md">
      <h2 class="text-lg font-semibold mb-4">Modifier le lycée</h2>

      <label class="block mb-2 text-sm font-medium text-gray-700">
        Sélectionnez un lycée :
      </label>

      <div class="relative">
        <select
          v-model="selectedLycee"
          class="custom-select"
        >
          <option v-for="(lycee, index) in allLycees" :key="index" :value="lycee">
            {{ lycee }}
          </option>
        </select>
      </div>

      <div class="flex justify-end mt-6 gap-3">
        <button class="btn btn-white" @click="closeDialog">Annuler</button>
        <button class="btn btn-black" @click="saveLycee">Sauvegarder</button>
      </div>
    </div>
  </div>
</template>

<script setup lang="ts">
import { ref, defineProps, defineEmits } from 'vue'

const props = defineProps({
  isOpen: Boolean,
  allLycees: Array,
  currentLycee: String
})
const emit = defineEmits(['close', 'save'])

const selectedLycee = ref(props.currentLycee || '')

const closeDialog = () => emit('close')
const saveLycee = () => {
  emit('save', selectedLycee.value)
  closeDialog()
}
</script>

<style>
/* Select styling */
.custom-select {
  @apply w-full border border-gray-300 rounded-md px-3 py-2 bg-white text-gray-800 focus:outline-none focus:ring-2 focus:ring-black;
  max-height: 12rem; /* roughly fits 6 options visually */
  /* overflow-y: auto; */
}
</style>
