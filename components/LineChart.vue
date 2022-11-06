<script>
import { Line } from 'vue-chartjs'

export default {
  extends: Line,
  props: {
    selectedPrefectures: {
      type: Array,
      default: () => [],
    },
    dataSets: {
      type: Array,
      default: () => [],
    },
  },
  data() {
    return {
      chartData: {
        labels: [
          '1985',
          '1990',
          '1995',
          '2000',
          '2005',
          '2010',
          '2015',
          '2020',
          '2025',
          '2030',
          '2035',
          '2040',
          '2045',
        ],
        datasets: [],
      },
      options: {
        responsive: true,
        maintainAspectRatio: false,
      },
    }
  },
  watch: {
    selectedPrefectures: {
      handler() {
        const converDataSetsToJson = JSON.stringify(this.selectedPrefectures)
        localStorage.setItem('key_name', converDataSetsToJson)
      },
      deep: true,
    },
    dataSets: {
      handler() {
        this.chartData.datasets = this.dataSets
        this.renderChart(this.chartData, this.options)
      },
      immediate: false,
    },
  },
  mounted() {
    this.renderChart(this.chartData, this.options)
  },
}
</script>
