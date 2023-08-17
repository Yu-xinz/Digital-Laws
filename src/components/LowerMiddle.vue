<template>
  <div class="lower-middle">
    <div align="right" class="lower-button">
      <v-btn
        class="ma-2"
        :icon="icon"
        color="#f0f0f0"
        size="small"
        @click="toggleChart"
      ></v-btn>
    </div>
    <div id="chartContainer" style="width: 100%; height: 100%"></div>
  </div>
</template>

<style scoped>
.lower-middle {
  flex: 1; /* 占据 1/6 的高度 */
  width: 900px;
  background-color: #faf8f8; /* 灰色背景 */
  box-shadow: 2px 2px 4px rgba(0, 0, 0, 0.2);
  position: relative;
}

.lower-button {
  position: absolute;
  width: 100%;
  right: 0;
  top: 0;
  z-index: 114;
}

.chart-container {
  position: relative;
  height: 100%;
}

.chart-toggle-button {
  position: relative;
  bottom: 20px;
}
</style>

<script lang="ts">
import * as echarts from 'echarts'
import scamData from '@/assets/output.json'

let filteredData = []

scamData.forEach((item) => {
  const year = item['裁判日期'].slice(0, 4)
  if (!filteredData[year]) {
    filteredData[year] = []
  }
  const later = item['裁判日期'].slice(5)
  filteredData[year].push([`2000-${later}`, Number(year) + Math.random() * 0.5])
})

console.log(filteredData['2010'])

export default {
  data() {
    return {
      chartType: 'scatter',
      chartData: null,
      icon: 'mdi-poll',
      myChart: null // 新增
    }
  },
  mounted() {
    this.initChart()
  },
  methods: {
    initChart() {
      const chartDom = document.getElementById('chartContainer')
      this.myChart = echarts.init(chartDom) // 更新 myChart 的值
      const calculateAverage = (data, dim) => {
        return data.length
      }
      const option = {
        title: {
          text: '妇女儿童失踪案件时间分布',
          subtext: '数据跨度：2010-01-01 ~ 2021-07-31'
        },
        grid: {
          left: '3%',
          right: '7%',
          bottom: '7%',
          containLabel: true
        },
        tooltip: {
          // trigger: 'axis',
          showDelay: 0,
          formatter: (params) => {
            if (params.value.length > 1) {
              return (
                params.seriesName.slice(0, 4) + '-' + params.value[0].slice(5)
              )
            } else {
              return (
                params.seriesName +
                ' :<br/>' +
                params.name +
                ' : ' +
                params.value +
                'kg '
              )
            }
          },
          axisPointer: {
            show: true,
            type: 'cross',
            label: {
              formatter: (params) =>
                params.axisDimension === 'x'
                  ? new Date(params.value).getMonth() + 1
                  : parseInt(params.value)
            },
            lineStyle: {
              type: 'dashed',
              width: 1
            }
          }
        },
        toolbox: {
          show: false,
          feature: {
            dataZoom: {},
            brush: {
              type: ['rect', 'polygon', 'clear']
            }
          }
        },
        brush: {},
        legend: {
          data: [
            '2010年',
            '2011年',
            '2012年',
            '2013年',
            '2014年',
            '2015年',
            '2016年',
            '2017年',
            '2018年',
            '2019年',
            '2020年',
            '2021年'
          ],
          left: 'center',
          bottom: 10
        },
        xAxis: [
          {
            type: 'time',
            scale: true,
            axisLabel: {
              formatter: (value) => new Date(value).getMonth() + 1,
              showMaxLabel: false
            },
            splitLine: {
              show: false
            }
          }
        ],
        yAxis: [
          {
            type: 'value',
            scale: true,
            splitLine: {
              show: false
            }
          }
        ],
        series: [
          {
            name: '2010年',
            type: 'scatter',
            itemStyle: {
              opacity: 0.6
            },
            symbolSize: 8,
            emphasis: {
              focus: 'series'
            },
            // prettier-ignore
            data: filteredData["2010"],
            markArea: {
              silent: true,
              itemStyle: {
                color: 'transparent',
                borderWidth: 1,
                borderType: 'dashed'
              },
              data: [
                [
                  {
                    name: '2010',
                    xAxis: 'min',
                    yAxis: 'min'
                  },
                  {
                    xAxis: 'max',
                    yAxis: 'max'
                  }
                ]
              ]
            }
          },
          {
            name: '2011年',
            type: 'scatter',
            itemStyle: {
              opacity: 0.6
            },
            symbolSize: 8,
            emphasis: {
              focus: 'series'
            },
            // prettier-ignore
            data: filteredData["2011"],
            markArea: {
              silent: true,
              itemStyle: {
                color: 'transparent',
                borderWidth: 1,
                borderType: 'dashed'
              },
              data: [
                [
                  {
                    name: '2011',
                    xAxis: 'min',
                    yAxis: 'min'
                  },
                  {
                    xAxis: 'max',
                    yAxis: 'max'
                  }
                ]
              ]
            }
          },
          {
            name: '2012年',
            type: 'scatter',
            itemStyle: {
              opacity: 0.6
            },
            symbolSize: 8,
            emphasis: {
              focus: 'series'
            },
            // prettier-ignore
            data: filteredData["2012"],
            markArea: {
              silent: true,
              itemStyle: {
                color: 'transparent',
                borderWidth: 1,
                borderType: 'dashed'
              },
              data: [
                [
                  {
                    name: '2012',
                    xAxis: 'min',
                    yAxis: 'min'
                  },
                  {
                    xAxis: 'max',
                    yAxis: 'max'
                  }
                ]
              ]
            }
          },
          {
            name: '2013年',
            type: 'scatter',
            itemStyle: {
              opacity: 0.6
            },
            symbolSize: 8,
            emphasis: {
              focus: 'series'
            },
            // prettier-ignore
            data: filteredData["2013"],
            markArea: {
              silent: true,
              itemStyle: {
                color: 'transparent',
                borderWidth: 1,
                borderType: 'dashed'
              },
              data: [
                [
                  {
                    name: '2013',
                    xAxis: 'min',
                    yAxis: 'min'
                  },
                  {
                    xAxis: 'max',
                    yAxis: 'max'
                  }
                ]
              ]
            }
          },
          {
            name: '2014年',
            type: 'scatter',
            itemStyle: {
              opacity: 0.6
            },
            symbolSize: 8,
            emphasis: {
              focus: 'series'
            },
            // prettier-ignore
            data: filteredData["2014"],
            markArea: {
              silent: true,
              itemStyle: {
                color: 'transparent',
                borderWidth: 1,
                borderType: 'dashed'
              },
              data: [
                [
                  {
                    name: '2014',
                    xAxis: 'min',
                    yAxis: 'min'
                  },
                  {
                    xAxis: 'max',
                    yAxis: 'max'
                  }
                ]
              ]
            }
          },
          {
            name: '2015年',
            type: 'scatter',
            itemStyle: {
              opacity: 0.6
            },
            symbolSize: 8,
            emphasis: {
              focus: 'series'
            },
            // prettier-ignore
            data: filteredData["2015"],
            markArea: {
              silent: true,
              itemStyle: {
                color: 'transparent',
                borderWidth: 1,
                borderType: 'dashed'
              },
              data: [
                [
                  {
                    name: '2015',
                    xAxis: 'min',
                    yAxis: 'min'
                  },
                  {
                    xAxis: 'max',
                    yAxis: 'max'
                  }
                ]
              ]
            }
          },
          {
            name: '2016年',
            type: 'scatter',
            itemStyle: {
              opacity: 0.6
            },
            symbolSize: 8,
            emphasis: {
              focus: 'series'
            },
            // prettier-ignore
            data: filteredData["2016"],
            markArea: {
              silent: true,
              itemStyle: {
                color: 'transparent',
                borderWidth: 1,
                borderType: 'dashed'
              },
              data: [
                [
                  {
                    name: '2016',
                    xAxis: 'min',
                    yAxis: 'min'
                  },
                  {
                    xAxis: 'max',
                    yAxis: 'max'
                  }
                ]
              ]
            }
          },
          {
            name: '2017年',
            type: 'scatter',
            itemStyle: {
              opacity: 0.6
            },
            symbolSize: 8,
            emphasis: {
              focus: 'series'
            },
            // prettier-ignore
            data: filteredData["2017"],
            markArea: {
              silent: true,
              itemStyle: {
                color: 'transparent',
                borderWidth: 1,
                borderType: 'dashed'
              },
              data: [
                [
                  {
                    name: '2017',
                    xAxis: 'min',
                    yAxis: 'min'
                  },
                  {
                    xAxis: 'max',
                    yAxis: 'max'
                  }
                ]
              ]
            }
          },
          {
            name: '2018年',
            type: 'scatter',
            itemStyle: {
              opacity: 0.6
            },
            symbolSize: 8,
            emphasis: {
              focus: 'series'
            },
            // prettier-ignore
            data: filteredData["2018"],
            markArea: {
              silent: true,
              itemStyle: {
                color: 'transparent',
                borderWidth: 1,
                borderType: 'dashed'
              },
              data: [
                [
                  {
                    name: '2018',
                    xAxis: 'min',
                    yAxis: 'min'
                  },
                  {
                    xAxis: 'max',
                    yAxis: 'max'
                  }
                ]
              ]
            }
          },
          {
            name: '2019年',
            type: 'scatter',
            itemStyle: {
              opacity: 0.6
            },
            symbolSize: 8,
            emphasis: {
              focus: 'series'
            },
            // prettier-ignore
            data: filteredData["2019"],
            markArea: {
              silent: true,
              itemStyle: {
                color: 'transparent',
                borderWidth: 1,
                borderType: 'dashed'
              },
              data: [
                [
                  {
                    name: '2019',
                    xAxis: 'min',
                    yAxis: 'min'
                  },
                  {
                    xAxis: 'max',
                    yAxis: 'max'
                  }
                ]
              ]
            }
          },
          {
            name: '2020年',
            type: 'scatter',
            itemStyle: {
              opacity: 0.6
            },
            symbolSize: 8,
            emphasis: {
              focus: 'series'
            },
            // prettier-ignore
            data: filteredData["2020"],
            markArea: {
              silent: true,
              itemStyle: {
                color: 'transparent',
                borderWidth: 1,
                borderType: 'dashed'
              },
              data: [
                [
                  {
                    name: '2020',
                    xAxis: 'min',
                    yAxis: 'min'
                  },
                  {
                    xAxis: 'max',
                    yAxis: 'max'
                  }
                ]
              ]
            }
          },
          {
            name: '2021年',
            type: 'scatter',
            itemStyle: {
              opacity: 0.6
            },
            symbolSize: 8,
            emphasis: {
              focus: 'series'
            },
            // prettier-ignore
            data: filteredData["2021"],
            markArea: {
              silent: true,
              itemStyle: {
                color: 'transparent',
                borderWidth: 1,
                borderType: 'dashed'
              },
              data: [
                [
                  {
                    name: '2021',
                    xAxis: 'min',
                    yAxis: 'min'
                  },
                  {
                    xAxis: 'max',
                    yAxis: 'max'
                  }
                ]
              ]
            }
          }
        ]
      }
      const barOption = {
        title: {
          text: '妇女儿童失踪案件时间分布',
          subtext: '数据跨度：2010-01-01 ~ 2021-07-31'
        },
        grid: {
          left: '3%',
          right: '7%',
          bottom: '7%',
          containLabel: true
        },
        tooltip: {
          showDelay: 0,
        },
        xAxis: {
          type: 'category',
          data: [
            '2010',
            '2011',
            '2012',
            '2013',
            '2014',
            '2015',
            '2016',
            '2017',
            '2018',
            '2019',
            '2020',
            '2021'
          ]
        },
        yAxis: {},
        series: [
          {
            type: 'bar',
            id: 'total',
            data: [
              {
                value: calculateAverage(filteredData['2010'], 0),
                groupId: '2010'
              },
              {
                value: calculateAverage(filteredData['2011'], 0),
                groupId: '2011'
              },
              {
                value: calculateAverage(filteredData['2012'], 0),
                groupId: '2012'
              },
              {
                value: calculateAverage(filteredData['2013'], 0),
                groupId: '2013'
              },
              {
                value: calculateAverage(filteredData['2014'], 0),
                groupId: '2014'
              },
              {
                value: calculateAverage(filteredData['2015'], 0),
                groupId: '2015'
              },
              {
                value: calculateAverage(filteredData['2016'], 0),
                groupId: '2016'
              },
              {
                value: calculateAverage(filteredData['2017'], 0),
                groupId: '2017'
              },
              {
                value: calculateAverage(filteredData['2018'], 0),
                groupId: '2018'
              },
              {
                value: calculateAverage(filteredData['2019'], 0),
                groupId: '2019'
              },
              {
                value: calculateAverage(filteredData['2020'], 0),
                groupId: '2020'
              },
              {
                value: calculateAverage(filteredData['2021'], 0),
                groupId: '2021'
              }
            ],
            universalTransition: {
              enabled: true,
              seriesKey: ['female', 'male'],
              delay: function (idx, count) {
                return Math.random() * 400
              }
            }
          }
        ]
      }

      this.chartData = {
        scatter: option,
        bar: barOption
      }

      this.updateChart()
    },
    updateChart() {
      const option = this.chartData[this.chartType]
      console.log('jb toggle:', option)
      this.myChart.setOption(option, true) // 使用 this.myChart 更新图表
    },
    toggleChart() {
      console.log('Before toggle:', this.chartType)
      this.chartType = this.chartType === 'scatter' ? 'bar' : 'scatter'
      console.log('After toggle:', this.chartType)
      this.icon =
        this.chartType === 'scatter' ? 'mdi-poll' : 'mdi-chart-scatter-plot'
      this.updateChart()
    }
  }
}
</script>
