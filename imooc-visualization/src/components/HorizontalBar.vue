<template>
  <div>【大区数据信息】</div>
  <div ref="target" class="h-1/3 box-border pb-4">横向柱状图</div>
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
//  1.初始化echarts实例
let mChart = null;
const target = ref(null);
onMounted(() => {
  mChart = echarts.init(target.value);
  renderChart();
});
// 2.构建option对象
const renderChart = () => {
  const options = {
    // x轴的展示数据
    xAxis: {
      show: false,
      type: "value",
      max: function (value) {
        return parseInt(value.max * 1.2);
      },
    },
    tooltip: {
      trigger: "axis",
      axisPointer: {
        type: "shadow",
      },
    },
    // y轴展示数据
    yAxis: {
      type: "category",
      data: props.data.regions.map((item) => item.name),
      // 反向
      inverse: true,
      // 不展示轴线
      axisLine: {
        show: false,
      },
      // 不展示刻度
      axisTick: {
        show: false,
      },
      //  修该文字颜色
      axisLabel: {
        color: "#9EB1C8",
      },
    },
    // echart图位置
    grid: {
      top: 0,
      right: 0,
      bottom: 0,
      left: 0,
      containLabel: true,
    },
    // 图片核心
    series: [
      {
        // 图形类型
        type: "bar",
        data: props.data.regions.map((item) => ({
          name: item.name,
          value: item.value,
        })),
        // 柱状图背景
        showBackground: true,
        backgroundStyle: {
          color: "rgba(180, 180, 180, 0.2)",
        },
        // 轴样式
        itemStyle: {
          color: "#479AD3",
          // 柱圆角
          barBorderRadius: 10,
          // 阴影配置
          shadowColor: "rgba(0,0,0,0.2)",
          // 阴影模糊度配置
          shadowBlur: 5,
          // 字体设置
        },
        barWidth: 12,
        label: {
          show: true,
          position: "right",
          textStyle: {
            color: "#fff",
          },
        },
      },
    ],
  };
  // 3.步
  mChart.setOption(options);
};
// 监听数据变化,变化后触发renderChart
watch(
  () => props.data,
  () => {
    renderChart();
  }
);
</script>

<style></style>
