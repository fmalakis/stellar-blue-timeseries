<template>
  <main class="bg-slate-700 p-4 min-h-screen">
    <section class="bg-slate-600 rounded-lg p-4 ">
        <h1 class="text-white font-bold text-2xl md:lg:text-3xl text-center md:lg:text-start">Timeseries Analyzer and Editor</h1>
        <h2 class="text-white text-sm md:lg:text-xl mt-2">This is a demo project for Stellar Blue's technical interview. You can use this demo tool to:</h2>
        <div class="flex flex-row justify-around">
        </div>
    </section>
    <section class="mt-4 bg-slate-600 p-4 rounded-lg">
      <h2 class="text-white text-2xl font-semibold">Filters:</h2>
      <div class="flex flex-col md:flex-row items-start md:items-center justify-start gap-4 mt-2">
        <div>
          <label for="startDate" class="text-white mr-2 text-xl">After:</label>
          <input
            type="date"
            id="startDate"
            v-model="startDate"
            class="p-2 text-white bg-gray-700 rounded-lg border border-gray-400 dark:text-white dark:[color-scheme:dark]"
          />
        </div>
        <div>
          <label for="endDate" class="text-white mr-2 text-xl">Before:</label>
          <input
            type="date"
            id="endDate"
            v-model="endDate"
            class="p-2 text-white bg-gray-700 rounded-lg border border-gray-400 dark:text-white dark:[color-scheme:dark]" 
          />
        </div>
        <div>
            <input
                type="checkbox"
                id="showDE"
                v-model="showDE"
                class="accent-purple-600 size-4"
            />
            <label for="showDE" class="text-white text-xl ml-2">Germany</label>
        </div>
        <div>
            <input
                type="checkbox"
                id="showGR"
                v-model="showGR"
                class="accent-purple-600 size-4"
            />
            <label for="showGR" class="text-white text-xl ml-2">Greece</label>
        </div>
        <div>
            <input
                type="checkbox"
                id="showFR"
                v-model="showFR"
                class="accent-purple-600 size-4"
            />
            <label for="showFR" class="text-white text-xl ml-2">France</label>
        </div>
      </div>
    </section>
    <section class="mt-4 grid lg:grid-cols-2 grid-cols-1 gap-4">
        <div class="bg-slate-600 rounded-lg p-4">
            <h3 class="text-white font-bold text-xl md:lg:text-2xl">Electricity Prices</h3>
            <PricesTable v-if="filteredData" :prices="filteredData" :showDE="showDE" :showGR="showGR" :showFR="showFR" @editModalClicked="openModal"/>
        </div>
        <div class="bg-slate-600 rounded-lg p-4">
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
        console.log(row, country, entry, date)
        this.currentRow = row;
        this.currentCol = country;
        this.currentVal = entry;
        this.currentDate = date;
        this.isEditModalShowing = true;
    },
    handleModalClose(newValue) {
        if (newValue) this.updateCell(this.currentRow, this.currentCol, newValue)
        this.currentRow = null;
        this.currentCol = null;
        this.currentVal = null;
        this.currentDate = null;
        this.isEditModalShowing = false;
    }
  },
  data() {
    return {
       currentRow: null,
       currentCol: null,
       currentVal: null,
       currentDate: null,
    }
  },
  setup() {

    const rawData = ref([]);
    const startDate = ref("");
    const endDate = ref("");
    const showDE = ref(true);
    const showGR = ref(true);
    const showFR = ref(true);
    const isEditModalShowing = ref(false);


    const formatTime = (dateString) => new Date(dateString).toLocaleDateString("en-DE", { year: 'numeric', month: 'numeric', day: 'numeric', hour: 'numeric', minute: 'numeric' });


    onMounted(async () => {
      try {
        const response = await fetch("src/assets/timeseries.json");
        if (!response.ok) throw new Error(`JSON error! Status: ${response.status}`);
        rawData.value = await response.json();
      } catch (error) {
        console.error("Error loading data from JSON:", error);
      }
    });

    const formattedData = computed(() => {
      return rawData.value.map((entry) => ({
        date: formatTime(entry.DateTime),
        timestamp: new Date(entry.DateTime).getTime(),
        DE: parseFloat(entry.ENTSOE_DE_DAM_Price),
        GR: parseFloat(entry.ENTSOE_GR_DAM_Price),
        FR: parseFloat(entry.ENTSOE_FR_DAM_Price),
      }));
    });

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