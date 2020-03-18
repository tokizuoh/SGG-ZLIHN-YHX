<template>
  <div>
    <h1>都道府県一覧</h1>  
    <div v-if="!errored">
      <div
        v-for="pref in prefectures"
        :key="pref.prefCode">
        <input 
          :id="pref.prefName"
          v-model="checkedPrefCodes" 
          :value="pref.prefCode" 
          type="checkbox">
        <label :for="pref.Name">{{ pref.prefName }}</label>
      </div>
      <div>
        <button @click="getPopulations">send</button>
        <button @click="showData">show</button>
      </div>
      <div>
        <graph 
          :graphdata="populations"/>
      </div>
    </div>
    <div v-else>
      {{ info.data.statusCode }} Error
    </div>
  </div>
</template>

<script>
import accessToken from './accessToken.json'
import Graph from './Graph'

export default {
  name: 'App',
  components: {
    Graph: Graph
  },
  data () {
    return {
      info: null,
      loading: true,
      errored: false,
      prefectures: null,
      checkedPrefCodes: [],
      populations: [] 
    }
  },
  mounted () {
    this.axios
      .get('https://opendata.resas-portal.go.jp/api/v1/prefectures', {
        headers: {
          'X-API-KEY': accessToken['RESAS_API_KEY']
        }
      })
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
  },
  methods: {
    showData: function () {
      console.log(this.populations)
    },
    getPopulation: function (prefCode){
      this.axios
        .get('https://opendata.resas-portal.go.jp/api/v1/population/composition/perYear', {
          headers: { 
            'X-API-KEY': accessToken['RESAS_API_KEY']
          },
          params: {
            'prefCode': prefCode,
            'cityCode': '-'
          }
        })
        .then(response => {
          this.populations[prefCode] = response.data.result.data['0'].data
        })
        .catch(error => {
          console.log(error)
          this.errored = true
        })
        .finally(() => this.loading = false)
    },
    getPopulations: function() {
      for (let i = 0; i < this.checkedPrefCodes.length; i++) {
        this.getPopulation(this.checkedPrefCodes[i])
      }
    }
  }
}
</script>
