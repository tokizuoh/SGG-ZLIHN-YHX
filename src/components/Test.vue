<template>
  <div>
    <h1>Bitcoin Price Index</h1>
    <!--     
    <div
      v-for="currency in info"
      :key="currency.code"
      class="currency">
      {{ currency.description }}
      hoge
      <span class="lighten">
        <span v-html="currency.symbol"/>{{ currency.rate_float | currencydecimal }}
      </span>
    </div>-->
    {{ info }}
  </div>
</template>

<script>
import accessToken from './accessToken.json'
export default {
  name: 'App',
  data () {
    return {
      info: null,
      loading: true,
      errored: false,
    }
  },
  mounted () {
    this.axios
      .get('https://opendata.resas-portal.go.jp/api/v1/prefectures', { headers: { 'X-API-KEY': accessToken['RESAS_API_KEY']}})
      .then(response => {
        // 正常に値をgetできる場合はレスポンスに'statusCode'キーが存在しない
        if (response['data']['statusCode'] != null){
          console.log(response['data']['statusCode'], 'error')
          return
        }
        this.info = response
      })
      .catch(error => {
        console.log(error)
        this.errored = true
      })
      .finally(() => this.loading = false)
  }
}
</script>
