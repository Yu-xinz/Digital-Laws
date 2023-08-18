<template>
  <div id="sankey" class="sidebar-left">
    <!-- 下半部分内容 -->
  </div>
</template>

<style scoped>
.sidebar-left {
  
  width: 400px;
  background-color: #faf8f8; /* 灰色背景 */
  margin-right: 20px; /* 留有空隙 */
  box-shadow: 2px 2px 4px rgba(0, 0, 0, 0.2);
}
</style>

<script setup lang="ts">
import * as echarts from 'echarts';
import { onMounted } from "vue";
onMounted(() => {
  var chartDom = document.getElementById('sankey');
  var myChart = echarts.init(chartDom);
  var option;

  option = {
    title: {
          text: '妇女儿童失踪案件被告人特征分析',
        },
    tooltip: {
      trigger: 'item',
      triggerOn: 'mousemove'
    },
    animation: false,
    series: [
      {
        type: 'sankey',
        emphasis: {
          focus: 'adjacency'
        },
        data: [
          {name: '男'},
          {name: '女'},
          {name: '壮族'},
          {name: '汉族'},
          {name: '彝族'},
          {name: '苗族'},
          {name: '初中文化'},
          {name: '小学文化'},
          {name: '无业'},
          {name: '文盲'},
          {name: '高中文化'},
          {name: '中专文化'},
          {name: '农民'},
          {name: '无职业'},
          {name: '务工'},
        ],
        links: [
                {'source': '男', 'target': '汉族', 'value': 657},
                {'source': '男', 'target': '苗族', 'value': 55},
                {'source': '男', 'target': '彝族', 'value': 18},
                {'source': '女', 'target': '汉族', 'value': 432},
                {'source': '女', 'target': '苗族', 'value': 20},
                {'source': '女', 'target': '壮族', 'value': 10},
                {'source': '女', 'target': '彝族', 'value': 14},
                {'source': '汉族', 'target': '小学文化', 'value': 417},
                {'source': '汉族', 'target': '初中文化', 'value': 248},
                {'source': '汉族', 'target': '高中文化', 'value': 29},
                {'source': '汉族', 'target': '中专文化', 'value': 20},
                {'source': '汉族', 'target': '文盲', 'value': 214},
                {'source': '汉族', 'target': '农民', 'value': 45},
                {'source': '苗族', 'target': '小学文化', 'value': 31},
                {'source': '苗族', 'target': '文盲', 'value': 25},
                {'source': '彝族', 'target': '文盲', 'value': 22},
                {'source': '壮族', 'target': '小学文化', 'value': 12},
                {'source': '小学文化', 'target': '务工', 'value': 26},
                {'source': '小学文化', 'target': '农民', 'value': 319},
                {'source': '小学文化', 'target': '无业', 'value': 54},
                {'source': '小学文化', 'target': '无职业', 'value': 19},
                {'source': '初中文化', 'target': '务工', 'value': 24},
                {'source': '初中文化', 'target': '无业', 'value': 30},
                {'source': '初中文化', 'target': '农民', 'value': 174},
                {'source': '文盲', 'target': '农民', 'value': 202},
                {'source': '文盲', 'target': '无业', 'value': 36},
                {'source': '文盲', 'target': '务工', 'value': 12},
                {'source': '高中文化', 'target': '农民', 'value': 15},
        ],
        orient: 'vertical',
        label: {
          position: 'top'
        },
        lineStyle: {
          color: 'source',
          curveness: 0.5
        }
      }
    ]
  };

  option && myChart.setOption(option);
});
</script>