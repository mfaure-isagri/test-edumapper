<template>
    <div class="page-container">
        <!-- Header -->
        <header class="page-header">
            <img src="/public/icons/image.png" alt="Logo" class="logo" />
        </header>

        <!-- Main Content -->
        <main class="page-main flex-col">
            <div class="main-container flex-col">
                <template v-if="step === 0">
                    <div class="gradient-card">
                        <span class="highschool">
                            {{ selectedLycee }}
                        </span>
                        <div class="highschool-infos">
                            <span class=" flex flex-row justify-center items-center gap-1">
                                <MapPinIcon  class="w-3 h-3 text-white" />
                                {{ location }}
                            </span>
                            <span class=" flex flex-row justify-center items-center gap-1">
                                <BuildingOffice2Icon class="w-3 h-3 text-white" />
                                {{ type }}
                            </span>
                        </div>
                        <button class="btn btn-white" @click="isDialogOpen = true">Modifier</button>
                    </div>
                    <!-- <highschoolCard
                        :openDialogLycee="openDialogLycee()"
                        :selectedLycee="selectedLycee"
                        :location="location"
                        :type="type"
                    /> -->
    
                    <div class="options-selector">
                        <div class="title-row flex flex-row items-center" @click="isOpenClass = !isOpenClass">
                            <span class="text-lg font-semibold">Classe</span>
                            <div style="margin-left: auto; gap: 5px;" class="flex flex-row justify-center item-center">
                                <span v-if="!classValid">A compléter</span>
                                <chevron-down-icon class="h-3 w-3 chevron" style="margin: auto;" :class="{ '3s rotate-180': isOpenClass }"/>
                            </div>
                        </div>
                        <div class="option-content" v-if="isOpenClass">
                            <div class="grid grid-cols-auto-fit gap-3 my-4">
                                <div
                                    v-for="(item, index) in classOptions.year"
                                    :key="index"
                                    @click="classChoice.year = item"
                                    class="selectable-item"
                                    :class="{ 'selected': classChoice.year === item }"
                                >
                                    {{ item }}
                                </div>
                            </div>
                            <hr class="border-t-2 border-gray-300 my-4" />
                            <span class="text-sm">Type de bac</span>
                            <div class="grid grid-cols-auto-fit gap-3 my-4">
                                <div
                                    v-for="(item, index) in classOptions.type"
                                    :key="index"
                                    @click="classChoice.type = item"
                                    class="selectable-item"
                                    :class="{ 'selected': classChoice.type === item }"
                                >
                                    {{ item }}
                                </div>
                            </div>
                            <button 
                                class="btn btn-black full" 
                                :class="{ 'btn-disabled': !isClassValid }"
                                :disabled="!isClassValid"
                                @click="isOpenClass = !isOpenClass"
                            >
                                Confimer
                            </button>
                        </div>
                    </div>
                    <div class="options-selector">
                        <div class="title-row flex flex-row items-center" @click="isOpenSpecialty = !isOpenSpecialty">
                            <span class="text-lg font-semibold">Spécialités</span>
                            <div style="margin-left: auto; gap: 5px;" class="flex flex-row justify-center item-center">
                                <span>A compléter</span>
                                <chevron-down-icon class="h-3 w-3 chevron" style="margin: auto;" :class="{ '3s rotate-180': isOpenSpecialty }"/>
                            </div>
                        </div>
                    </div>
                    
                    <button 
                        class="btn btn-black" 
                        style="margin-top: auto; width: 100%;"
                        :class="{ 'btn-disabled': !isClassValid }"
                        :disabled="!isClassValid"
                        @click="nextStep()"
                    >
                        Confirmer
                    </button>
                </template>
                <template v-if="step == 1">
                    <div class="gradient-card">
                        <span class="highschool">
                            {{ selectedLycee }}
                        </span>
                        <div class="highschool-infos">
                            <span class=" flex flex-row justify-center items-center gap-1">
                                <MapPinIcon  class="w-3 h-3 text-white" />
                                {{ location }}
                            </span>
                            <span class=" flex flex-row justify-center items-center gap-1">
                                <BuildingOffice2Icon class="w-3 h-3 text-white" />
                                {{ type }}
                            </span>
                        </div>
                        <div class="highschool-infos">
                            <span class=" flex flex-row justify-center items-center gap-1">
                                <AcademicCapIcon  class="w-3 h-3 text-white" />
                                {{ classChoice.year }}
                            </span>
                            <span class=" flex flex-row justify-center items-center gap-1">
                                <AcademicCapIcon class="w-3 h-3 text-white" />
                                {{ classChoice.type }}
                            </span>
                        </div>
                        <button class="btn btn-white" @click="step = 0">Modifier</button>
                    </div>

                    <importFileCard @file-selected="handleFile" />

                    <div class="flex items-center">
                        <InformationCircleIcon class="w-[1em] h-[1em]"/>
                        Transmettre ta fiche Avenir permet d'affiner le résultat de tes chances d'admission.
                    </div>
                    
                    <button 
                        class="btn btn-black" 
                        style="margin-top: auto; width: 100%;"
                        @click="nextStep()"
                    >
                        Suivant
                    </button>

                    <a
                    href="https://www.parcoursup.gouv.fr/faq/thematiques/autres-sujets/fiche-avenir"
                    target="_blank"
                    rel="noopener noreferrer"
                    class="btn btn-white"
                    >
                        Je n'ai pas de fiche Avenir pour cette formation
                    </a>
                </template>

                <template v-if="step === 3">

                </template>

            </div>
        </main>
        
        <editLyceeModal
            :isOpen="isDialogOpen"
            :allLycees="allLycees"
            :currentLycee="selectedLycee"
            @close="isDialogOpen = false"
            @save="updateLycee"
        />
    </div>
</template>

<script>

import { BuildingOffice2Icon, MapPinIcon, ChevronDownIcon, AcademicCapIcon, InformationCircleIcon } from '@heroicons/vue/24/outline'
import editLyceeModal from './components/editLyceeModal.vue';
import highschoolCard from './components/highschoolCard.vue';
import importFileCard from './components/importFileCard.vue';

export default {
    components: {
        BuildingOffice2Icon, 
        MapPinIcon ,
        AcademicCapIcon,
        InformationCircleIcon,
        ChevronDownIcon,
        editLyceeModal,
        importFileCard,
    },
    data() {
        return {
            selectedLycee: '',
            allLycees: [],
            location: 'Lille',
            type: 'lycée public',
            class: '',
            specialty: '',
            step: 0,
            loading: false,
            isOpenClass: false,
            isOpenSpecialty: false,
            isDialogOpen: false,
            classChoice: {
                year: "",
                type: ""
            },
            classOptions: {
                year: [
                    "Seconde",
                    "Première",
                    "Terminale"
                ],
                type: [
                    "Général",
                    "Technologique",
                    "Professionnel"
                ],
                specialty: [
                    'HGGSP',
                    'HLP',
                    'LLCE',
                    'LCA',
                    'Maths',
                    'NSI',
                    'PC',
                    'SVT',
                    'SI',
                    'SES',
                    'EPS',
                    'Arts',
                    'BE',
                ]
            }
        }
    },

    async mounted() {
        await this.fetchRandomLycee()
    },

    computed: {
        isClassValid() {
            return (
                this.classOptions.year.includes(this.classChoice.year) &&
                this.classOptions.type.includes(this.classChoice.type)
            )
        }
    },

    methods: {
        nextStep() {
            if (this.step <= 1) {
                this.step += 1
            }
        },
        handleFile(file) {
            console.log('File imported:', file)
        },
        async fetchRandomLycee() {
            this.loading = true
            try {
                const data = await $fetch('/api/random-info')
                this.selectedLycee = data.lycee
                this.allLycees = data.allLycees
            } catch (error) {
                console.error('Erreur lors du chargement :', error)
            } finally {
                this.loading = false
            }
        },
        updateLycee(newLycee) {
            this.selectedLycee = newLycee
        },
        openDialogLycee() {
            this.isDialogOpen = true;
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
  @apply flex-1 flex items-center p-6 ;
  .main-container {
    @apply flex-1 flex items-center w-full;
    max-width: 800px;
    margin: 0 auto;
    gap: 15px;
  }
}

.grid-cols-auto-fit {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(120px, 1fr));
}
.options-selector {
    @apply w-full h-max flex flex-col rounded-2xl shadow-lg bg-white;
    padding: 20px;
    gap: 10px;
    .selectable-item {
        @apply cursor-pointer text-center px-4 py-2 transition-colors duration-300 text-sm font-medium;
        @apply bg-gray-200 text-gray-800 hover:bg-gray-300;
        border-radius: 20px;
        min-width: 120px;
    }

    .selectable-item.selected {
        @apply bg-white text-black shadow;
    }
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
    font-size: 0.9em;
    &.full{
        @apply w-full;
    }
}

.btn-disabled {
  @apply bg-white text-gray-400 border border-gray-300 cursor-not-allowed hover:bg-white; 
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

.chevron {
  @apply transform transition-transform duration-500 ease-[cubic-bezier(0.4,0,0.2,1)];
}
</style>
