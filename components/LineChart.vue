<script>
import { Line } from 'vue-chartjs'

export default {
  extends: Line,
  data() {
    return {
      chartData: {
        labels: ['', 'January', 'February', 'y', 'h', 'j', 'i', 'l'],
        datasets: [
          {
            type: 'line',
            data: [0, 10, 60, 20, 40, 50, 10, 20],
            label: '人口推移',
            borderWidth: 3,
            fill: false,
            lineTension: 0.1,
            order: 1,
            borderColor: 'green',
          },
          {
            type: 'line',
            data: [0, 100, 10, 50, 10, 90, 10, 20],
            label: '人口推移',
            borderWidth: 3,
            fill: false,
            lineTension: 0.1,
            order: 1,
            borderColor: 'pink',
            pointBackgroundColor: 'green',
          },
        ],
      },
      options: {
        responsive: true,
        maintainAspectRatio: true,
      },
    }
  },
  mounted() {
    this.renderChart(this.chartData, this.options)
  },
}
</script>
