<template>
  <div class="Prefectures">
    <div>{{ selectedPrefuctures }}</div>
    <div>
      <v-btn depressed color="info"> 全解除 </v-btn>
    </div>
    <div class="Prefectures_PrefectureWrapper">
      <template v-for="(prefecture, index) in prefectures">
        <div :key="index" class="Prefectures_Prefecture">
          <v-checkbox
            v-model="selectedPrefuctures"
            :label="prefecture.prefName"
            color="info"
            :value="prefecture.prefName"
            hide-details
          ></v-checkbox>
        </div>
      </template>
    </div>
  </div>
</template>

<script>
export default {
  name: 'PopulationTransitionAppIndex',
  data() {
    return {
      test: 'yohei',
      selectedPrefuctures: [],
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

  methods: {
    allClear() {
      this.selectedPrefuctures = []
    },
  },
}
</script>

<style lang="scss" scoped>
.Prefectures_PrefectureWrapper {
  width: 90%;
  margin: 0 auto;
  margin-top: 50px;
  display: grid;
  grid-template-columns: 1fr 1fr 1fr 1fr 1fr;
}
</style>
