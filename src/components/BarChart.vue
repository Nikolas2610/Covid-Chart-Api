<template>
    <div>
        <Bar :chart-options="chartOptions" :chart-data="chartData" :chart-id="chartId" :dataset-id-key="datasetIdKey"
            :plugins="plugins" :css-classes="cssClasses" :styles="styles" :width="width" :height="height" />
    </div>
</template>

<script>
import { Bar } from 'vue-chartjs'
import { Chart as ChartJS, Title, Tooltip, Legend, BarElement, CategoryScale, LinearScale } from 'chart.js'

ChartJS.register(Title, Tooltip, Legend, BarElement, CategoryScale, LinearScale)

export default {
    name: 'BarChart',
    components: { Bar },
    props: {
        color: {
            type: String
        },
        label: {
            type: String
        },
        chart: {
            type: Array
        },
        chartId: {
            type: String,
            default: 'bar-chart'
        },
        datasetIdKey: {
            type: String,
            default: 'label'
        },
        width: {
            type: Number,
            default: 400
        },
        height: {
            type: Number,
            default: 400
        },
        cssClasses: {
            default: '',
            type: String
        },
        styles: {
            type: Object,
            default: () => { }
        },
        plugins: {
            type: Array,
            default: () => []
        }
    },
    data() {
        return {
            chartData: {
                labels: ['January', 'February', 'March'],
                datasets: [{ data: [40, 20, 12] }],
            },
            chartOptions: {
                responsive: true,
                maintainAspectRatio: false
            },
        }
    },
    methods: {
        async setData() {
            const dates = this.chart.map(d => d.date).reverse();
            const totals = this.chart.map(d => d.total).reverse();
            this.chartData.labels = dates;
            this.chartData.datasets = [{ data: totals, label: this.label, backgroundColor: this.color }];
        }
    },
    mounted() {
        this.setData();
    }
}
</script>