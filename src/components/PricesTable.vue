<template>
    <div class="max-h-96 overflow-x-auto">
      <table class="table-auto w-full mt-4">
        <thead class="sticky top-0">
          <tr class="bg-cyan-700 text-white text-md md:text-l lg:text-xl">
            <th>Date</th>
            <th v-if="showDE">Germany (DE)</th>
            <th v-if="showGR">Greece (GR)</th>
            <th v-if="showFR">France (FR)</th>
          </tr>
        </thead>
        <tbody class="bg-white text-sm md:lg:text-l">
          <tr v-for="(entry, index) in prices" :key="index">
            <td>{{ entry.date }}</td>
            <td v-if="showDE">
                <span class="inline-flex items-center gap-2">{{ entry.DE }}
                    <button @click="openModal(index, 'DE', entry.DE, entry.date)" type="button" class="text-white bg-blue-700 hover:bg-blue-800 focus:ring-4 focus:outline-none focus:ring-blue-300 font-medium rounded-lg text-sm p-1 text-center inline-flex items-end me-2 dark:bg-blue-600 dark:hover:bg-blue-700 dark:focus:ring-blue-800 cursor-pointer">
                        <svg xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke-width="1.5" stroke="currentColor" class="size-2.5 md:lg:size-4">
                        <path stroke-linecap="round" stroke-linejoin="round" d="m16.862 4.487 1.687-1.688a1.875 1.875 0 1 1 2.652 2.652L10.582 16.07a4.5 4.5 0 0 1-1.897 1.13L6 18l.8-2.685a4.5 4.5 0 0 1 1.13-1.897l8.932-8.931Zm0 0L19.5 7.125M18 14v4.75A2.25 2.25 0 0 1 15.75 21H5.25A2.25 2.25 0 0 1 3 18.75V8.25A2.25 2.25 0 0 1 5.25 6H10" />
                        </svg>
                    </button>
                </span>
            </td>
            <td v-if="showGR">
                <span class="inline-flex items-center gap-2">{{ entry.GR }} 
                    <button @click="openModal(index, 'GR', entry.GR, entry.date)" type="button" class="text-white bg-blue-700 hover:bg-blue-800 focus:ring-4 focus:outline-none focus:ring-blue-300 font-medium rounded-lg text-sm p-1 text-center inline-flex items-end me-2 dark:bg-blue-600 dark:hover:bg-blue-700 dark:focus:ring-blue-800 cursor-pointer">
                        <svg xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke-width="1.5" stroke="currentColor" class="size-2.5 md:lg:size-4">
                        <path stroke-linecap="round" stroke-linejoin="round" d="m16.862 4.487 1.687-1.688a1.875 1.875 0 1 1 2.652 2.652L10.582 16.07a4.5 4.5 0 0 1-1.897 1.13L6 18l.8-2.685a4.5 4.5 0 0 1 1.13-1.897l8.932-8.931Zm0 0L19.5 7.125M18 14v4.75A2.25 2.25 0 0 1 15.75 21H5.25A2.25 2.25 0 0 1 3 18.75V8.25A2.25 2.25 0 0 1 5.25 6H10" />
                        </svg>
                    </button>
                </span>
            </td>
            <td v-if="showFR">
                <span class="inline-flex items-center gap-2">{{ entry.FR }} 
                    <button @click="openModal(index, 'FR', entry.FR, entry.date)" type="button" class="text-white bg-blue-700 hover:bg-blue-800 focus:ring-4 focus:outline-none focus:ring-blue-300 font-medium rounded-lg text-sm p-1 text-center inline-flex items-end me-2 dark:bg-blue-600 dark:hover:bg-blue-700 dark:focus:ring-blue-800 cursor-pointer">
                        <svg xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke-width="1.5" stroke="currentColor" class="size-2.5 md:lg:size-4">
                        <path stroke-linecap="round" stroke-linejoin="round" d="m16.862 4.487 1.687-1.688a1.875 1.875 0 1 1 2.652 2.652L10.582 16.07a4.5 4.5 0 0 1-1.897 1.13L6 18l.8-2.685a4.5 4.5 0 0 1 1.13-1.897l8.932-8.931Zm0 0L19.5 7.125M18 14v4.75A2.25 2.25 0 0 1 15.75 21H5.25A2.25 2.25 0 0 1 3 18.75V8.25A2.25 2.25 0 0 1 5.25 6H10" />
                        </svg>
                    </button>
                </span>
            </td>
          </tr>
        </tbody>
      </table>
    </div>
  </template>
  
<script>
  export default {
    props: {
        prices: Array,
        showDE: Boolean,
        showGR: Boolean,
        showFR: Boolean
    },
    methods: {
        openModal(index, country, entry, date) {
            this.$emit('editModalClicked', index, country, entry, date)
        }
    }
  };
</script>
  
<style scoped>
  th, td {
    border: 1px solid #ddd;
    padding: 8px;
    text-align: center;
  }
</style>
  