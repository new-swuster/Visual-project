<template>
  <div>【服务资源占用比】</div>
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

const renderChart = () => {
  const options = {
    xAxis: {
      type: "category",
      data: props.data.servers.map((item) => item.name),
      axisLabel: {
        color: "#9EB1C8",
      },
    },
    tooltip: {
      trigger: "axis",
      formatter: "{c}%",
      axisPointer: {
        type: "shadow",
      },
    },
    yAxis: {
      type: "value",
      show: false,
      max: function (value) {
        return parseInt(value.max * 1.2);
      },
    },
    grid: {
      top: 15,
      right: 0,
      bottom: 20,
      left: -2,
      // 柱间距
      containLabel: true,
    },
    series: [
      {
        type: "bar",
        data: props.data.servers.map((item) => ({
          name: item.name,
          value: item.value,
        })),
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
          // y轴字体位置
          position: "top",
          textStyle: {
            color: "#fff",
          },
          // 对数据修改
          formatter: "{c}%",
        },
      },
    ],
  };
  mChart.setOption(options);
};
watch(() => props.data, renderChart);
</script>
<style></style>
