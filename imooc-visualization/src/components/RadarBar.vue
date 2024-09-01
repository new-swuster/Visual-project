<template>
  <div>【云端报警风险】</div>
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
    // 雷达图坐标系配置
    tooltip: {
      show: true,
    },
    grid: {
      top: 0,
      right: 0,
      bottom: 0,
      left: 0,
      containLabel: true,
    },
    radar: {
      name: {
        textStyle: {
          color: "#05D5FF",
          fontSize: 15,
        },
      },
      // 雷达图形状
      shape: "polygon",
      // 位置
      center: ["50%", "50%"],
      // 圆滑度
      radius: "80%",
      // 起始角度
      startAngle: 120,
      // 轴线配置
      axisLine: {
        lineStyle: {
          color: "rgba(5,213,255,0.5)",
        },
      },
      // 网格线配置
      splitLine: {
        show: true,
        lienStyle: {
          width: 2,
          color: "rgba(5,213,255,0.5)",
        },
      },
      // 指示器对应文字
      indicator: props.data.risks.map((item) => ({
        name: item.name,
        max: 99,
      })),
      // 区域是否拆分
      splitArea: {
        show: false,
      },
    },
    // 坐标系的极点
    polar: {
      center: ["50%", "50%"],
      redius: "0%",
    },
    // 坐标角度
    angleAxis: {
      min: 0,
      // 设置分割线间隔
      interval: 10,
      // 数据增张方向
      clockwise: false,
      // 不显示坐标轴刻度
      axisTick: {
        show: false,
      },
      // 不显示坐标轴文字
      axisLabel: {
        show: false,
      },
      splitLine: {
        show: false,
      },
    },
    // 径向轴
    radiusAxis: {
      min: 0,
      interval: 25,
      splitLine: {
        show: false,
      },
    },
    // 核心
    series: {
      type: "radar",
      symbol: "circle",
      symbolSize: 10,
      itemStyle: {
        normal: {
          color: "#05D5FF",
        },
      },
      // 填充区域
      areaStyle: {
        normal: {
          color: "#05D5FF",
          opacity: 0.5,
        },
      },
      // 线条颜色
      lineStyle: {
        width: 3,
        color: "#05D5FF",
      },
      label: {
        normal: {
          show: true,
          color: "#fff",
        },
      },
      // 数据
      data: [
        {
          value: props.data.risks.map((item) => item.value),
        },
      ],
    },
  };
  mChart.setOption(options);
};
watch(() => props.data, renderChart);
</script>
<style></style>
