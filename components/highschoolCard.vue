<template>
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
        <button class="btn btn-white" @click="$emit('openDialogLycee')">Modifier</button>
    </div>
</template>

<script>

import { BuildingOffice2Icon, MapPinIcon } from '@heroicons/vue/24/outline'

export default {
    components: {
        BuildingOffice2Icon, 
        MapPinIcon ,
    },
    props: ['selectedLycee', 'location', 'type', 'isDialogOpen'],

}
</script>

<style>

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
</style>
