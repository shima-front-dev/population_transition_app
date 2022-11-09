<script>
import { Line } from 'vue-chartjs'

export default {
  extends: Line,
  props: {
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
    dataSets: {
      handler() {
        const convertDataSetsToJson = JSON.stringify(this.dataSets)
        localStorage.setItem('dataSets', convertDataSetsToJson)
        const getDataSetsFromLocalStrage = JSON.parse(
          localStorage.getItem('dataSets')
        )
        this.chartData.datasets = getDataSetsFromLocalStrage
        this.renderChart(this.chartData, this.options)
      },
      immediate: false,
    },
  },
  mounted() {
    const getDataSetsFromLocalStrage = JSON.parse(
      localStorage.getItem('dataSets')
    )
    this.chartData.datasets = getDataSetsFromLocalStrage
    this.renderChart(this.chartData, this.options)
  },
}
</script>
