<template>
  <div>
    <h1>都道府県一覧</h1>  
    <div v-if="!errored">
      <div
        v-for="pref in prefectures"
        :key="pref.prefCode">
        <input 
          :id="pref.prefName"
          v-model="checkedNames" 
          :value="pref.prefName" 
          type="checkbox">
        <label :for="pref.Name">{{ pref.prefName }}</label>
      </div>
      {{ checkedNames }}
    </div>
    <div v-else>
      {{ info.data.statusCode }} Error
    </div>
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
      prefectures: null,
      checkedNames: []
    }
  },
  mounted () {
    this.axios
      .get('https://opendata.resas-portal.go.jp/api/v1/prefectures', { headers: { 'X-API-KEY': accessToken['RESAS_API_KEY']}})
      .then(response => {
        // 正常に値をgetできる場合はレスポンスに'statusCode'キーが存在しない
        if (response['data']['statusCode'] != null){
          console.log(response['data']['statusCode'], 'error')
          this.errored = true
        }
        this.info = response
        this.prefectures = this.info.data.result
      })
      .catch(error => {
        console.log(error)
        this.errored = true
      })
      .finally(() => this.loading = false)
  }
}
</script>
