<template>
  <div
    class="bg-[url('assets/imgs/bg.jpg')] bg-cover bg-center h-screen text-white p-5 flex overflow-hidden"
  >
    <!-- 左 -->
    <div class="flex-1 mr-5 bg-opacity-50 bg-slate-800 p-3flex flex-col">
      <!-- 横向柱状图 -->
      <HorizontalBar
        class="h-1/3 box-border pb-4"
        :data="data.regionData"
      ></HorizontalBar>
      <!-- 雷达图 -->
      <RadarBar class="h-1/3 box-border pd-4" :data="data.riskData"></RadarBar>
      <!-- 关系图 -->
      <Relation
        class="h-1/3 box-border pd-4 bg-slate-800"
        :data="data.relationData"
      ></Relation>
    </div>
    <!-- 中 -->
    <div class="w-1/2 mr-5 flex flex-col">
      <!-- 数据总览图 -->
      <TotalData
        class="bg-opacity-50 bg-slate-900 p-3"
        :data="data.totalData"
      ></TotalData>
      <!-- 地图可视化 -->
      <MapChart
        class="bg-opacity-50 p-3 mt-4 flex-1"
        :data="data.mapData"
      ></MapChart>
    </div>
    <!-- 右 -->
    <div class="flex-1 mr-5 bg-opacity-50 bg-slate-800 p-3flex flex-col">
      <!-- 竖向柱状图 -->
      <VerticalBar
        class="h-1/3 box-border pd-4"
        :data="data.serverData"
      ></VerticalBar>
      <!-- 环形图 -->
      <RingBar
        class="h-1/3 box-border pd-4"
        :data="data.abnormalData"
      ></RingBar>
      <!-- 文档云图 -->
      <WordCloud
        class="h-1/3 box-border"
        :data="data.wordCloudData"
      ></WordCloud>
    </div>
  </div>
</template>

<script setup>
// 导入组件
import HorizontalBar from "./components/HorizontalBar.vue";
import MapChart from "./components/MapChart.vue";
import RadarBar from "./components/RadarBar.vue";
import Relation from "./components/Relation.vue";
import RingBar from "./components/RingBar.vue";
import TotalData from "./components/TotalData.vue";
import VerticalBar from "./components/VerticalBar.vue";
import WordCloud from "./components/WordCloud.vue";
import { ref } from "vue";
import { getVisualization } from "./api/visualization.js";
// 获取数据
const data = ref(null);
const loadData = async () => {
  data.value = await getVisualization();
}
// 数据动态变1.4秒
setInterval(() => {
  loadData();
}, 1400);
</script>
