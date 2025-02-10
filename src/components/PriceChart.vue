<template>
    <div>
        <LineChart ref="chartRef" v-if="chartData" :chart-data="chartData" :chart-options="chartOptions" />
    </div>
</template>

<script>
import { computed, ref, watch } from 'vue';
import { LineChart } from 'vue-chart-3';
import { Chart, registerables } from 'chart.js';

    Chart.register(...registerables);
    Chart.defaults.color = '#fff';

    export default {
    components: {
        LineChart
    },
    props: {
        prices: Array,
        showDE: Boolean,
        showGR: Boolean,
        showFR: Boolean
    },
    setup(props) {
        const chartRef = ref(null);

        // Create a computed property that builds chartData from the prop
        const chartData = computed(() => {
            if (!props.prices || props.prices.length === 0) {
                return null;
            }

            const labels = props.prices.map(entry => entry.date);

            // Build separate arrays for each country’s prices
            const dePrices = props.prices.map(entry => entry.DE);
            const grPrices = props.prices.map(entry => entry.GR);
            const frPrices = props.prices.map(entry => entry.FR);

            return {
                labels,
                datasets: [
                {
                    label: 'Germany (DE) Price',
                    data: dePrices,
                    borderColor: 'blue',
                    backgroundColor: 'rgba(0, 0, 255, 0.2)',
                    tension: 0.3,
                },
                {
                    label: 'Greece (GR) Price',
                    data: grPrices,
                    borderColor: 'green',
                    backgroundColor: 'rgba(0, 255, 0, 0.2)',
                    tension: 0.3,
                },
                {
                    label: 'France (FR) Price',
                    data: frPrices,
                    borderColor: 'red',
                    backgroundColor: 'rgba(255, 0, 0, 0.2)',
                    tension: 0.3,
                },
                ],
            };
        });

        // Define the chart options
        const chartOptions = computed(() => ({
            responsive: true,
            maintainAspectRatio: true,
            plugins: {
                legend: {
                position: 'top',
                labels: {
                    color: '#fff',
                },
                },
                tooltip: {
                titleColor: '#fff',
                bodyColor: '#fff',
                },
            },
            scales: {
                x: {
                title: { display: true, text: 'Time', color: '#fff' },
                ticks: { color: '#fff' },
                grid: { color: 'rgba(255,255,255,0.2)' },
                },
                y: {
                title: { display: true, text: 'Price (EUR/MWh)', color: '#fff' },
                ticks: { color: '#fff' },
                grid: { color: 'rgba(255,255,255,0.2)' },
                },
            },
        }));

        // Hides/shows timeseries when filters on HomeView change
        const updateVisibility = () => {
            // Ensure the chart instance exists.
            if (!chartRef.value || !chartRef.value.chartInstance) {
                return;
            }
            const chart = chartRef.value.chartInstance;

            // Dataset indices: 0 -> DE, 1 -> GR, 2 -> FR.
            chart.getDatasetMeta(0).hidden = !props.showDE;
            chart.getDatasetMeta(1).hidden = !props.showGR;
            chart.getDatasetMeta(2).hidden = !props.showFR;


            chart.update();
        };

        // Watch for changes in the visibility props and update the chart.
        watch(
            () => [props.showDE, props.showGR, props.showFR],
            updateVisibility,
            { immediate: true }
        );

        return { chartData, chartOptions, chartRef };
    },
    };
</script>
  