<template>
  <div>【数据传递信息】</div>
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
    tooltip: {
      show: true,
    },
    xAxis: {
      show: false,
      type: "value",
    },
    yAxis: {
      show: false,
      type: "value",
    },
    series: [
      {
        type: "graph",
        layout: "none",
        coordinateSystem: "cartesian2d",
        symbolSize: 26,
        z: 2,
        edgeLabel: {
          normal: {
            show: true,
            color: "#fff",
            textStyle: {
              fontSize: 14,
            },
            formatter: function (params) {
              return params.data.speed;
            },
          },
        },
        label: {
          normal: {
            show: true,
            position: "bottom",
            color: "#5E5E5E",
          },
        },
        edgeSymbol: ["none", "arrow"],
        edgeSymbolSize: 8,
        data: props.data.relations.map((item) => {
          if (item.id !== 0) {
            return {
              name: item.name,
              category: 0,
              active: true,
              speed: "${item.speed}kb/s",
              value: item.value,
            };
          } else {
            return {
              name: item.name,
              value: item.value,
              symbolSize: 100,
              itemStyle: {
                normal: {
                  color: {
                    //渐变颜色
                    colorStops: [
                      {
                        offset: 0,
                        color: "#157eff",
                      },
                      {
                        offset: 1,
                        color: "#35c2ff",
                      },
                    ],
                  },
                },
              },
              label: {
                normal: {
                  fontSize: 14,
                },
              },
            };
          }
        }),
        // 配置数据关系
        links: props.data.relations.map((item, index) => ({
          source: item.source,
          target: item.target,
          speed: `${item.speed}kb/s`,
          lineStyle: {
            normal: {
              color: "#12b5d0",
              curveness: 0.2,
            },
          },
          label: {
            show: true,
            position: "middle",
            // 偏移量
            offset: [10, 10, 0],
          },
        })),
      },
      {
        type: "lines",
        coordinateSystem: "cartesian2d",
        z: 1,
        // 线样式配置
        effect: {
          show: true,
          smooth: false,
          trailLength: 0,
          symbol: "arrow",
          color: "rgba(55,155,255,0.5)",
          symbolSize: 12,
        },
        lineStyle: {
          normal: {
            curveness: 0.2,
          },
        },
        data: [
          [{ coord: [0, 300] }, { coord: [50, 200] }],
          [{ coord: [0, 100] }, { coord: [50, 200] }],
          [{ coord: [50, 200] }, { coord: [100, 100] }],
          [{ coord: [50, 200] }, { coord: [100, 300] }],
        ],
      },
    ],
  };
  mChart.setOption(options);
};
watch(() => props.data, renderChart);
</script>
<style></style>
