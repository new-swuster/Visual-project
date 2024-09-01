<template>
  <div>
    <div ref="target" class="w-full h-full"></div>
  </div>
</template>
<script setup>
import { ref, onMounted, watch } from "vue";
import * as echarts from "echarts";
import mapJson from "../assets/MapData/china.json";
const props = defineProps({
  data: {
    type: Object,
    required: true,
  },
});

const target = ref(null);
let mChart = null;
onMounted(() => {
  // 注册地图
  echarts.registerMap("china", mapJson);
  mChart = echarts.init(target.value);
  renderChart();
});

const renderChart = () => {
  let options = {
    tooltip: {
      show: true,
    },
    // 开发时间轴
    timeline: {
      data: props.data.voltageLevel,
      axisType: "category",
      // 时间线是否自动切换
      autoPlay: true,
      // 切换时间
      playInterval: 1350,
      // 位置
      left: "10%",
      right: "10%",
      bottom: "0%",
      width: "80%",
      label: {
        normal: {
          textStyle: {
            color: "#ddd",
          },
        },
        // 高亮配置
        emphasis: {
          textStyle: {
            color: "#fff",
          },
        },
      },
      // 字体大小
      symbolSize: 10,
      // 线颜色
      lineStyle: {
        color: "#8b8888",
      },
      // 选中点的颜色
      checkpointStyle: {
        borderColor: "#fff",
        borderWidth: 2,
      },
      // 空键样式
      controlStyle: {
        // 下
        showNextBtn: true,
        // 上
        showPrevBtn: true,
        normal: {
          color: "#666",
          borderColor: "#666",
        },
        // 高亮样式
        emphasis: {
          color: "#aaa",
          borderColor: "#aaa",
        },
      },
    },
    // 柱状图
    baseOption: {
      grid: {
        right: "2%",
        top: "15%",
        bottom: "10%",
        width: "20%",
      },
      // 地图配置
      geo: {
        tooltip: {
          show: true,
        },
        show: true,
        map: "china",
        // 缩放
        zoom: 1.0,
        roam: true,
        // 中心位置
        center: [113.83531246, 34.0267395887],
        // 省份样式
        itemStyle: {
          normal: {
            borderColor: "rgba(147,235,248,1)",
            borderWidth: 1,
            // 区域颜色
            areaColor: {
              type: "radial",
              x: 0.5,
              y: 0.5,
              r: 0.5,
              // 渐变色配置
              colorStops: [
                {
                  offset: 0,
                  color: "rgba(147,235,248, 0)",
                },
                {
                  offset: 1,
                  color: "rgba(147,235,248,0.2)",
                },
              ],
            },
          },
          // 高亮显示
          emphasis: {
            areaColor: "#389BB7",
            borderWidth: 0,
          },
        },
      },
    },
    // 让柱状图谁时间轴变化而变化
    options: [],
  };
  // 循环数据传入options中
  props.data.voltageLevel.forEach((item, index) => {
    options.options.push({
      backgroundColor: "#142037",
      tooltip: {
        show: true,
      },
      title: [
        // 主标题，对应地图
        {
          text: "2019-2023 年度数据统计",
          left: "0%",
          top: "0",
          textStyle: {
            color: "#ccc",
            fontSize: 30,
          },
        },
        // 副标题，对应柱形图
        {
          id: "statistic",
          text: item + "年数据统计情况",
          right: "0%",
          top: "4%",
          textStyle: {
            color: "#ccc",
            fontSize: 20,
          },
        },
      ],
      xAxis: {
        type: "value",
        scale: true,
        position: "top",
        splitLine: {
          show: false,
        },
        axisLine: {
          show: false,
        },
        axisTick: {
          show: false,
        },
        axisLable: {
          margin: 2,
          textStyle: {
            color: "#aaa",
          },
        },
      },
      yAxis: {
        type: "category",
        axisLine: {
          show: true,
          lineStyle: {
            color: "#ddd",
          },
        },
        axisTick: {
          show: false,
        },
        axisLabel: {
          interval: 0,
          textStyle: {
            color: "#ddd",
          },
        },
        data: props.data.categoryData[item].map((item) => item.name),
      },
      series: [
        {
          type: "bar",
          zlevel: 1.5,
          itemStyle: {
            normal: {
              // 动态变化
              color: props.data.colors[index],
            },
          },
          data: props.data.categoryData[item].map((item) => item.value),
        },
        // 散点图配置
        {
          type: "effectScatter",
          coordinateSystem: "geo",
          data: props.data.topData[item],
          symbolSize: function (val) {
            return val[2] / 4;
          },
          // 效果配置
          showEffectOn: "render",
          rippleEffect: {
            brushType: "stroke",
          },
          label: {
            normal: {
              formatter: "{b}",
              position: "right",
              show: true,
            },
          },
          itemStyle: {
            normal: {
              color: props.data.colors[index],
              shadowBlur: 5,
              shadoColor: props.data.colors[index],
            },
          },
        },
      ],
    });
  });
  mChart.setOption(options);
};
watch(() => props.data, renderChart);
</script>
<style>
.s {
  background-color: #8b8888;
}
</style>
