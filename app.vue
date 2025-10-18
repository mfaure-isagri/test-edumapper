<template>
    <div class="page-container">
        <!-- Header -->
        <header class="page-header">
            <img src="/public/icons/image.png" alt="Logo" class="logo" />
        </header>

        <!-- Main Content -->
        <main class="page-main">
            <div class="gradient-card">
                <span class="highschool">
                    {{ this.lycee }}
                </span>
                <div class="highschool-infos">
                    <span class=" flex flex-row justify-center items-center gap-1">
                        <MapPinIcon  class="w-3 h-3 text-white" />
                        {{ this.location }}
                    </span>
                    <span class=" flex flex-row justify-center items-center gap-1">
                        <BuildingOffice2Icon class="w-3 h-3 text-white" />
                        {{ this.type }}
                    </span>
                </div>
                <button class="btn btn-white">Modifier</button>
            </div>
        </main>
    </div>
</template>

<script>

import { BuildingOffice2Icon, MapPinIcon } from '@heroicons/vue/24/outline'

export default {
    components: {
        BuildingOffice2Icon, 
        MapPinIcon ,
    },
    data() {
        return {
            lycee: '',
            allLycees: [],
            location: 'Lille',
            type: 'lycée public',
            loading: false,
        }
    },

    async mounted() {
        await this.fetchRandomLycee()
    },

    methods: {
        async fetchRandomLycee() {
            this.loading = true
            try {
                const data = await $fetch('/api/random-info')
                this.lycee = data.lycee
                this.allLycees = data.allLycees
            } catch (error) {
                console.error('Erreur lors du chargement :', error)
            } finally {
                this.loading = false
            }
        }
    }
}
</script>

<style>
.page-container {
  @apply min-h-screen flex flex-col bg-[#F7F3F0];
}

.page-header {
  @apply flex items-center p-4 shadow;
}

.logo {
  @apply h-10 w-auto;
}

.page-main {
  @apply flex-1 flex justify-center p-6;
}

.gradient-card {
  @apply w-full h-max flex flex-col rounded-2xl shadow-lg text-white ;
  background: linear-gradient(120deg, #FF7342 50%, #B176FF);
  padding: 20px;
  .highschool{
    @apply text-lg font-semibold;
    padding-bottom: 5px;
  };
  .highschool-infos {
    @apply text-sm flex flex-row;
    padding-bottom: 20px;
    gap:15px
  }
}
.btn {
    @apply px-4 py-2 w-max  font-medium transition-colors duration-200 border;
    border-radius: 30px;
    font-size: 0.9em
}

/* Color variants */
.btn-white {
    @apply bg-white text-gray-800 border-gray-300 hover:bg-gray-100;
}

.btn-gray {
    @apply bg-gray-200 text-gray-800 border-gray-300 hover:bg-gray-300;
}

.btn-black {
    @apply bg-black text-white border-black hover:bg-gray-800;
}

</style>
