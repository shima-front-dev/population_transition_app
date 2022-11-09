<template>
  <div class="Prefectures">
    <div class="PrefecturesWrapper">
      <h2>都道府県別人口推移</h2>
      <button class="allClearButton" @click="allClear">全解除</button>
      <div class="Prefectures_CheckboxWrapper">
        <template v-for="(prefecture, index) in prefectures">
          <div :key="index" class="Prefectures_CheckboxContent">
            <div>
              <input
                type="checkbox"
                :value="prefecture.prefCode"
                v-model="selectedPrefectures"
                @change="
                  getDemographics(prefecture.prefCode, prefecture.prefName)
                "
              />
            </div>
            <div>
              <label>{{ prefecture.prefName }}</label>
            </div>
          </div>
        </template>
      </div>
      <div class="Prefectures_PrefectureChartWrapper">
        <div>
          <LineChart :data-sets="dataSets" />
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import LineChart from '@/components/LineChart.vue'
import { COLORS } from '@/constants/colors'
export default {
  name: 'PopulationTransitionAppIndex',
  components: {
    LineChart,
  },
  data() {
    return {
      selectedPrefectures: [],
      dataSets: [],
    }
  },
  computed: {},
  async asyncData({ $axios, $config }) {
    const url = 'https://opendata.resas-portal.go.jp/api/v1/prefectures'
    const response = await $axios.$get(url, {
      headers: { 'X-API-KEY': $config.apiKey },
    })
    return {
      prefectures: response.result,
    }
  },
  watch: {
    selectedPrefectures: {
      handler() {
        const convertSelectedPrefecturesToJson = JSON.stringify(
          this.selectedPrefectures
        )
        localStorage.setItem(
          'selectedPrefectures',
          convertSelectedPrefecturesToJson
        )
      },
      deep: true,
    },
  },
  mounted() {
    const getSelectedPrefecturesFromLocalStrage = JSON.parse(
      localStorage.getItem('selectedPrefectures')
    )
    this.selectedPrefectures = getSelectedPrefecturesFromLocalStrage
    const getDataSetsFromLocalStrage = JSON.parse(
      localStorage.getItem('dataSets')
    )
    this.dataSets = getDataSetsFromLocalStrage
  },
  methods: {
    allClear() {
      this.selectedPrefectures = []
      this.dataSets = []
    },
    async getDemographics(code, name) {
      if (!this.selectedPrefectures.includes(code)) {
        const filteredDataSets = this.dataSets.filter(
          (item) => item.id !== code
        )
        this.dataSets = filteredDataSets
      } else {
        const url =
          'https://opendata.resas-portal.go.jp/api/v1/population/composition/perYear'
        const response = await this.$axios.$get(url, {
          headers: { 'X-API-KEY': this.$config.apiKey },
          params: { prefCode: code, cityCode: '-' },
        })

        const filteredData = response.result.data[0].data.map((item) => {
          return item.value
        })
        const obj = {
          id: code,
          type: 'line',
          data: filteredData,
          label: name,
          borderWidth: 2,
          fill: false,
          lineTension: 0.1,
          order: 1,
          borderColor: COLORS[code],
        }
        this.dataSets.push(obj)
      }
    },
  },
}
</script>

<style lang="scss" scoped>
h2 {
  margin-bottom: 30px;
}
.Prefectures {
  max-width: 1000px;
  margin: 0 auto;
}
.PrefecturesWrapper {
  width: 90%;
  margin: 0 auto;
  padding-top: 30px;
}
.Prefectures_CheckboxWrapper {
  margin-left: 10px;
  margin-top: 20px;
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(80px, 1fr));
}
.Prefectures_CheckboxContent {
  font-size: 10px;
  display: flex;
  align-items: center;
  font-size: 15px;
}
.Prefectures_PrefectureChartWrapper {
  margin-top: 50px;
}
.allClearButton {
  background: rgba(3, 60, 246, 0.623);
  color: white;
  padding: 7px 10px;
  border-radius: 5px;
}
</style>
