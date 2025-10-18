<template>
    <div class="import-card flex flex-col w-full items-center justify-center gap-4">
        <h2 class=" text-lg font-semibold">Importer ma fiche Avenir</h2>

        <label
        for="file-upload"
        class="upload-area flex flex-col items-center justify-center cursor-pointer"
        >
        <ArrowDownTrayIcon class="h-10 w-10" style="margin-bottom: 10px;"/>
        <span class=" text-sm">Choisir un fichier</span>
        <input
            id="file-upload"
            type="file"
            class="hidden"
            @change="handleFileUpload"
        />
        </label>

        <p v-if="fileName" class=" text-sm mt-2">Fichier sélectionné : <b>{{ fileName }}</b></p>
    </div>
</template>

<script>
import { ArrowDownTrayIcon } from '@heroicons/vue/24/outline';

export default {
  emits: ['file-selected'],

  components: {
    ArrowDownTrayIcon,
  },

  data() {
    return {
      fileName: ''
    }
  },
  methods: {
    handleFileUpload(event) {
      const file = event.target.files[0]
      if (file) {
        this.fileName = file.name
        this.$emit('file-selected', file)
      }
    }
  }
}
</script>

<style>
.import-card {
  @apply  p-6 rounded-2xl shadow-lg bg-white;
  /* background: linear-gradient(120deg, #FF7342 50%, #B176FF); */
}

.upload-area {
  @apply border-2 border-dashed border-white rounded-xl p-6 text-center transition-all duration-300 hover:bg-white/10;
}
</style>
