<template>
  <div>テスト</div>
</template>

<script>
export default {
  name: 'PopulationTransitionAppIndex',
  data() {
    return {
      test: 'yohei',
    }
  },
  computed: {},
  async asyncData({ $axios, $config }) {
    console.log($config.apiKey)
    const url = 'https://opendata.resas-portal.go.jp/api/v1/prefectures'
    const response = await $axios.$get(url, {
      headers: { 'X-API-KEY': $config.apiKey },
    })
    return {
      posts: response,
    }
  },

  methods: {},
}
</script>

<style lang="scss" scoped></style>
