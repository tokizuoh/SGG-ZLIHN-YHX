<template>
  <div>
    <highcharts :options="lineData" />
    <button @click="setData">children</button>
    <button @click="makeGraph">Make</button>
    <button @click="showData">show</button>
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
  // watch: {
  //   graphdata: function (newValue, oldValue) {
  //     console.log(oldValue, '->', newValue)
  //   }
  // },
  mounted() {
    this.setLineData(this.defaultLineOptions())
  },
  methods: {
    defaultLineOptions() {
      return {
        type: '1',
      }
    },
    showData() {
      console.log('this.dictprefectures: ', this.dictprefectures)
    },
    makeGraph() {
      this.setLineData(this.defaultLineOptions())
    },
    setData() {
      this.fixData()
      this.lineData.series = this.newData
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
    // edit
    dataOptions(type, options) {
      const selected = type === options.type
      // const color = selected ? '#0066FF' : '#CDCDCD'
      return {
        // color,
        data: this.newData[type],
        lineWidth: selected ? 3 : 1,
        marker: {
          enabled: true,
          // fillColor: color,
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
          text: 'Title',
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
          useHTML: true,
          enabled: true,
          navigation: {
            enabled: false,
          },
        },
        xAxis: {
          title: {
            text: '年度'
          }
        },
        yAxis: {
          title: {
            text: '人口数'
          },
          min: 0,
          max: 10000000,
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
