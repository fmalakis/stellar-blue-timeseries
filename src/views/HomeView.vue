<template>
  <main class="dark:bg-slate-700 p-4 min-h-screen">
    <section class="bg-slate-500 dark:bg-slate-600 rounded-lg p-4 ">
        <h2 class="text-white text-xl md:lg:text-2xl mt-2">This is a demo project for Stellar Blue's technical interview. You can use this demo tool to:</h2>
        <div class="text-white pt-2 text-l md:lg:text-xl">
            <ul class="flex flex-col list-inside space-y-1">
                <li class="inline-flex items-center">
                    <svg xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke-width="1.5" stroke="currentColor" class="size-6 mr-1">
                        <path stroke-linecap="round" stroke-linejoin="round" d="M2.036 12.322a1.012 1.012 0 0 1 0-.639C3.423 7.51 7.36 4.5 12 4.5c4.638 0 8.573 3.007 9.963 7.178.07.207.07.431 0 .639C20.577 16.49 16.64 19.5 12 19.5c-4.638 0-8.573-3.007-9.963-7.178Z" />
                        <path stroke-linecap="round" stroke-linejoin="round" d="M15 12a3 3 0 1 1-6 0 3 3 0 0 1 6 0Z" />
                    </svg>
                    <span>View financial timeseries</span>
                </li>
                <li class="inline-flex items-center">
                    <svg xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke-width="1.5" stroke="currentColor" class="size-6 mr-1">
                        <path stroke-linecap="round" stroke-linejoin="round" d="M12 3c2.755 0 5.455.232 8.083.678.533.09.917.556.917 1.096v1.044a2.25 2.25 0 0 1-.659 1.591l-5.432 5.432a2.25 2.25 0 0 0-.659 1.591v2.927a2.25 2.25 0 0 1-1.244 2.013L9.75 21v-6.568a2.25 2.25 0 0 0-.659-1.591L3.659 7.409A2.25 2.25 0 0 1 3 5.818V4.774c0-.54.384-1.006.917-1.096A48.32 48.32 0 0 1 12 3Z" />
                    </svg>
                    <span>Filter based on date and country</span>
                </li>
                <li class="inline-flex items-center">
                    <svg xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke-width="1.5" stroke="currentColor" class="size-6 mr-2">
                        <path stroke-linecap="round" stroke-linejoin="round" d="m16.862 4.487 1.687-1.688a1.875 1.875 0 1 1 2.652 2.652L10.582 16.07a4.5 4.5 0 0 1-1.897 1.13L6 18l.8-2.685a4.5 4.5 0 0 1 1.13-1.897l8.932-8.931Zm0 0L19.5 7.125M18 14v4.75A2.25 2.25 0 0 1 15.75 21H5.25A2.25 2.25 0 0 1 3 18.75V8.25A2.25 2.25 0 0 1 5.25 6H10" />
                    </svg>
                    <span>Edit monetary value for a specific date and country</span>
                </li>
            </ul>   
        </div>
    </section>
    <section class="mt-4 bg-slate-500 dark:bg-slate-600 p-4 rounded-lg">
      <h2 class="text-white text-2xl font-semibold">Filters:</h2>
      <div class="flex flex-col md:flex-row items-start md:items-center justify-start gap-4 mt-2">
        <div>
          <label for="startDate" class="text-white mr-2 text-xl">After:</label>
          <input
            type="date"
            id="startDate"
            v-model="startDate"
            class="p-2 text-white bg-slate-600 dark:bg-gray-700 rounded-lg border border-gray-400 scheme-dark"
          />
        </div>
        <div>
          <label for="endDate" class="text-white mr-2 text-xl">Before:</label>
          <input
            type="date"
            id="endDate"
            v-model="endDate"
            class="p-2 text-white bg-slate-600 dark:bg-gray-700 rounded-lg border border-gray-400 scheme-dark" 
          />
        </div>
        <div class="flex flex-row flex-wrap gap-4">
            <div class="rounded-lg bg-purple-500 dark:bg-purple-600 inline-flex items-center p-2 cursor-pointer" :class="{ 'bg-purple-500/50 dark:bg-purple-600/50': !showDE }">
                <input
                    type="checkbox"
                    id="showDE"
                    v-model="showDE"
                    class="accent-purple-500 dark:accent-purple-600 size-4 cursor-pointer"
                />
                <label for="showDE" class="text-white text-xl ml-2 cursor-pointer">Germany</label>
            </div>
            <div class="rounded-lg bg-purple-500 dark:bg-purple-600 inline-flex items-center p-2 cursor-pointer" :class="{ 'bg-purple-500/50 dark:bg-purple-600/50': !showGR }">
                <input
                    type="checkbox"
                    id="showGR"
                    v-model="showGR"
                    class="accent-purple-500 dark:accent-purple-600 size-4 cursor-pointer"
                />
                <label for="showGR" class="text-white text-xl ml-2 cursor-pointer">Greece</label>
            </div>
            <div class="rounded-lg bg-purple-500 dark:bg-purple-600 inline-flex items-center p-2 cursor-pointer" :class="{ 'bg-purple-500/50 dark:bg-purple-600/50': !showFR }">
                <input
                    type="checkbox"
                    id="showFR"
                    v-model="showFR"
                    class="accent-purple-500 dark:accent-purple-600 size-4 cursor-pointer"
                />
                <label for="showFR" class="text-white text-xl ml-2 cursor-pointer">France</label>
            </div>
        </div>
      </div>
    </section>
    <section class="mt-4 grid xl:grid-cols-2 grid-cols-1 gap-4">
        <div class="bg-slate-500 dark:bg-slate-600 rounded-lg p-4">
            <h3 class="text-white font-bold text-xl md:lg:text-2xl">Electricity Prices</h3>
            <PricesTable v-if="filteredData" :prices="filteredData" :showDE="showDE" :showGR="showGR" :showFR="showFR" @editModalClicked="openModal"/>
        </div>
        <div class="bg-slate-500 dark:bg-slate-600 rounded-lg p-4">
            <h4 class="text-white font-bold text-xl md:lg:text-2xl">Electricity Prices over time</h4>
            <PriceChart v-if="filteredData" :prices="filteredData" :showDE="showDE" :showGR="showGR" :showFR="showFR"/>
        </div>
    </section>
    <EditModal v-if="isEditModalShowing" @modalClosed="handleModalClose" :currentValue="currentVal" :currentDate="currentDate" :currentCountry="currentCol"/>
  </main>
</template>

<script>
import { ref, onMounted, computed } from "vue";
import PricesTable from '@/components/PricesTable.vue'
import PriceChart from '@/components/PriceChart.vue'
import EditModal from '@/components/EditModal.vue'


export default {
  components: {
    PriceChart,
    PricesTable,
    EditModal
  },
  methods: {
    openModal(row, country, entry, date) {
        this.currentRow = row;
        this.currentCol = country;
        this.currentVal = entry;
        this.currentDate = date;
        this.isEditModalShowing = true;
    },
    handleModalClose(newValue) {
        if (newValue || newValue === 0) this.updateCell(this.currentRow, this.currentCol, newValue)
        this.currentRow = null;
        this.currentCol = null;
        this.currentVal = null;
        this.currentDate = null;
        this.isEditModalShowing = false;
    }
  },
  data() {
    return {
       // Currently selected row for cell edit
       currentRow: null,
       currentCol: null,
       currentVal: null,
       currentDate: null,
    }
  },
  setup() {

    const rawData = ref([]); // Raw data of the JSON file
    const startDate = ref("");
    const endDate = ref("");
    const showDE = ref(true);
    const showGR = ref(true);
    const showFR = ref(true);
    const isEditModalShowing = ref(false);

    // Format time based on project requirements
    const formatTime = (dateString) => new Date(dateString).toLocaleDateString("en-DE", { year: 'numeric', month: 'numeric', day: 'numeric', hour: 'numeric', minute: 'numeric' });


    onMounted(async () => {
      try {
        const response = await fetch("/timeseries.json");
        if (!response.ok) throw new Error(`JSON error! Status: ${response.status}`);
        rawData.value = await response.json();
      } catch (error) {
        console.error("Error loading data from JSON:", error);
      }
    });

    // Formatted data for better UX
    const formattedData = computed(() => {
      return rawData.value.map((entry) => ({
        date: formatTime(entry.DateTime),
        timestamp: new Date(entry.DateTime).getTime(),
        DE: parseFloat(entry.ENTSOE_DE_DAM_Price),
        GR: parseFloat(entry.ENTSOE_GR_DAM_Price),
        FR: parseFloat(entry.ENTSOE_FR_DAM_Price),
      }));
    });

    // Updates raw data on cell edit. Filtered data will then update since it is computed
    const updateCell = (rowIndex, column, newValue) => {
      if (!rawData.value[rowIndex]) return;
      if (column === "DE") {
        rawData.value[rowIndex].ENTSOE_DE_DAM_Price = newValue;
      } else if (column === "GR") {
        rawData.value[rowIndex].ENTSOE_GR_DAM_Price = newValue;
      } else if (column === "FR") {
        rawData.value[rowIndex].ENTSOE_FR_DAM_Price = newValue;
      }
    };

    // Filtered data for both table and chart
    const filteredData = computed(() => {
      if (!startDate.value && !endDate.value) {
        return formattedData.value;
      }

      // Convert provided dates to timestamps.
      const startTimestamp = startDate.value
        ? new Date(startDate.value).getTime()
        : -Infinity;
      // For the end date, we add nearly 24 hours (minus a millisecond) so that the entire day is included.
      const endTimestamp = endDate.value
        ? new Date(endDate.value).getTime() + 86399999
        : Infinity;

      // Filter the data based on the timestamp falling within the start and end bounds.
      return formattedData.value.filter(
        (item) =>
          item.timestamp >= startTimestamp && item.timestamp <= endTimestamp
      );
    });

    return { filteredData, startDate, endDate, showDE, showGR, showFR, isEditModalShowing, updateCell};
  },
};
</script>