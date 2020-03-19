<template>
  <div>
    <highcharts :options="lineData" />
    <button @click="makeGraph">Make</button>
  </div>
</template>

<script>
import { Chart } from 'highcharts-vue'

export default {
  components: {
    highcharts: Chart
  },
  props: {
    graphdata: {
      default: null,
      type: Array
    },
    dictprefectures: {
      default: null,
      type: Array
    }
  },
  data () {
    return {
      newData: [],
      lineData: {
        series: [
          {
            data: [1, 2, 3]
          },
          {
            data: [2, 3, 5]
          }
          ,
          {
            data: [4, 1, 0]
          }
        ]
      }
    }
  },
  mounted() {
    this.setLineData(this.defaultLineOptions())
  },
  methods: {
    defaultLineOptions() {
      return {
        type: '1',
      }
    },
    makeGraph() {
      this.fixData()
      this.lineData.series = this.newData
      this.setLineData(this.defaultLineOptions())
    },
    fixData() {
      for (let key in this.graphdata) {
        let arr = []
        for (let key2 in this.graphdata[key]) {
          arr.push(this.graphdata[key][key2].value)
        }
        this.$set(this.newData, key, arr)
        console.log(key, arr)
      }
    },
    dataOptions(type, options) {
      const selected = type === options.type
      return {
        data: this.newData[type],
        lineWidth: selected ? 3 : 1,
        marker: {
          enabled: true,
          radius: 6,
          symbol: 'circle',
        },
        name: this.dictprefectures[type],
        showInLegend: selected,
        zIndex: selected ? 1 : 0,
      }
    },
    setLineData (lineOptions) {
      this.lineData = {
        chart: {
          backgroundColor: 'transparent',
          borderColor: '#EEEEEEE',
          height: '300px',
          spacingRight: 20,
          type: 'line',
        },
        title: {
          align: 'left',
          margin: 24,
          style: {
            fontSize: '16px',
          },
          text: '都道府県別 総人口グラフ',
          x: 0,
        },
        legend: {
          align: 'top',
          itemStyle: {
            color: '#0066FF',
          },
          layout: 'vertical',
          symbolWidth: 40,
          verticalAlign: 'top',
          x: 72,
          y: -8,
        },
        xAxis: {
          title: {
            text: '年度'
          }
        },
        yAxis: {
          title: {
            text: '人口数'
          }
        },
        series: [],
      }
      for (let key in this.newData) {
        this.lineData.series.push(this.dataOptions(key, lineOptions))
      }
    }
  }
}
</script>
