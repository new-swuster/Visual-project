<template>
  <div>【大区异常处理】</div>
  <div ref="target" class="h-1/3 box-border pd-4"></div>
</template>
<script setup>
import { ref, onMounted, watch } from "vue";
import * as echarts from "echarts";
const props = defineProps({
  data: {
    type: Object,
    required: true,
  },
});

const target = ref(null);
let mChart = null;
onMounted(() => {
  mChart = echarts.init(target.value);
  renderChart();
});
// series配置有到的函数
const getSeriesData = () => {
  const series = [];
  // 数据遍历
  props.data.abnormals.forEach((item, index) => {
    // 上层
    series.push({
      name: item.name,
      type: "pie",
      clockWise: false,
      // 检测鼠标
      hoverAnimation: false,
      // 半径依次递减
      radius: [75 - index * 15 + "%", 70 - index * 15 + "%"],
      // 中心点
      center: ["55%", "55%"],
      label: {
        show: false,
      },
      data: [
        {
          value: item.value,
          name: item.name,
        },
        {
          // 最大数据
          value: 888,
          name: "",
          itemStyle: {
            color: "rgba(0, 0, 0, 0)",
            borderWidth: 0,
          },
          tooltip: {
            show: false,
          },
          hoverAnimation: false,
        },
      ],
    });
    // 下层
    series.push({
      name: item.name,
      type: "pie",
      // 事件无
      silent: true,
      z: 1,
      clockWise: false,
      hoverAnimation: false,
      radius: [75 - index * 15 + "%", 70 - index * 15 + "%"],
      center: ["55%", "55%"],
      label: {
        show: false,
      },
      data: [
        {
          value: 7.5,
          itemStyle: {
            color: "rgb(3,31,62)",
            borderWidth: 0,
          },
          tooltip: {
            show: false,
          },
          hoverAnimation: false,
        },
        {
          value: 2.5,
          itemStyle: {
            color: "rgba(0, 0, 0, 0)",
            borderWidth: 0,
          },
          tooltip: {
            show: false,
          },
          hoverAnimation: false,
        },
      ],
    });
  });
  return series;
};
const renderChart = () => {
  const options = {
    // 图例配置
    legend: {
      show: true,
      icon: "circle",
      top: "14%",
      left: "60%",
      data: props.data.abnormals.map((item) => item.name),
      width: -5,
      itemWidth: 11,
      itemHeight: 11,
      itemGap: 5,
      textStyle: {
        fontSize: 11,
        lineHight: 6,
        color: "#fff",
      },
    },
    // 提示层

    tooltip: {
      show: true,
      // 触发器
      trigger: "item",
      // 触发后展示内容
      formatter: "{b}:{c}({d}%)",
    },
    // 设置 y轴
    yAxis: [
      {
        type: "category",
        inverse: true,
        axisLine: {
          show: false,
        },
      },
    ],
    // 设置x轴
    xAxis: [
      {
        show: false,
      },
    ],
    series: getSeriesData(),
  };
  mChart.setOption(options);
};
watch(() => props.data, renderChart);
</script>
<style></style>
