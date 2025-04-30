<template>
  <div>
    <el-drawer v-model="drawer" direction="ltr" modal-class="info" size="100%"
      :title="`Hong Kong Walkability Scores - Hong Kong ${title}`" :modal="false" z-index="100">
      <!-- <div v-loading="loading">{{ info }}</div> -->
      <el-descriptions v-loading="loading" :column="1" border>
        <el-descriptions-item v-for="(value, key, index) in info" v-show="!['_id', 'coordinates'].includes(key)"
          :label="key" label-align="left" align="left" label-class-name="my-label" class-name="my-content"
          width="150px">{{ value }}</el-descriptions-item>
      </el-descriptions>
    </el-drawer>
    <div id="map"></div>
  </div>
</template>
<script setup>
import { Scene, PointLayer, LayerSwitch, RasterLayer } from '@antv/l7';
import { Map } from '@antv/l7-maps';
import { onMounted, ref } from 'vue';
import { data1, data2, data3, data4, data5, data6 } from '@/assets/data.js'
const drawer = ref(false)
const info = ref()
const loading = ref(false)
const title = ref('')
onMounted(() => {
  const scene = new Scene({
    id: 'map',
    map: new Map({
      center: [114.1480595, 22.33561081],
      zoom: 13,
      logo: false
    })
  });
  scene.on('loaded', () => {
      const layer = new RasterLayer();
    layer.source(
      'https://server.arcgisonline.com/ArcGIS/rest/services/World_Topo_Map/MapServer/tile/{z}/{y}/{x}',
      {
        parser: {
          type: 'rasterTile',
          tileSize: 256,
          minZoom: 6,
          maxZoom: 19,
        },
      },
    );
    scene.addLayer(layer);
    const pointLayer = new PointLayer({
      name: 'Vitality'
    })
      .source(data1, {
        parser: {
          type: 'json',
          x: 'longitude',
          y: 'latitude',
        },
      })
      .shape('simple')
      .size(5)
      .select({
        color: '#1677ff'
      })
      .scale('Vitality_score', {
        // .scale('Interactivityweighted_sum', {
        type: 'linear',
        domain: [0, 100]
      })
      .color('Vitality_score', [
        // .color('Interactivityweighted_sum', [
        '#FFFF00',
        '#FFE800',
        '#FFD100',
        '#FFBA00',
        '#FFA300',
        '#FF8C00',
        '#FF7500',
        '#FF5E00',
        '#FF4700',
        '#FF0000',
      ])
      .style({
        opacity: 1,
        strokeWidth: 0,
      });
    pointLayer.on('click', (e) => {
      loading.value = true
      drawer.value = true
      info.value = {}
      console.log(888, pointLayer)
      title.value = pointLayer.name
      setTimeout(() => {
        info.value = e.feature
        loading.value = false
      }, 1000)
      console.log(1234, e)
    })
    const point2Layer = new PointLayer({
      name: 'Consonance'
    })
      .source(data2, {
        parser: {
          type: 'json',
          
          x: 'longitude',
          y: 'latitude',
        },
      })
      .shape('simple')
      .size(5)
      .select({
        color: '#1677ff'
      })
      .scale('Consonance', {
        // .scale('Interactivityweighted_sum', {
        type: 'linear',
        domain: [0, 0.8]
      })
      .color('Consonance', [
        // .color('Interactivityweighted_sum', [
        '#FFFF00',
        '#FFE800',
        '#FFD100',
        '#FFBA00',
        '#FFA300',
        '#FF8C00',
        '#FF7500',
        '#FF5E00',
        '#FF4700',
        '#FF0000',
      ])
      .style({
        opacity: 1,
        strokeWidth: 0,
      });
      point2Layer.on('click', (e) => {
      loading.value = true
      drawer.value = true
      title.value = point2Layer.name
      info.value = {}
      setTimeout(() => {
        info.value = e.feature
        loading.value = false
      }, 1000)
      console.log(1234, e)
    })
    const point3Layer = new PointLayer({
      name: 'Interactivity'
    })
      .source(data3, {
        parser: {
          type: 'json',
          x: 'longitude',
          y: 'latitude',
        },
      })
      .shape('simple')
      .size(5)
      .select({
        color: '#1677ff'
      })
      .scale('Interactivity', {
        // .scale('Interactivityweighted_sum', {
        type: 'linear',
        domain: [0, .1]
      })
      .color('Interactivity', [
        // .color('Interactivityweighted_sum', [
        '#FFFF00',
        '#FFE800',
        '#FFD100',
        '#FFBA00',
        '#FFA300',
        '#FF8C00',
        '#FF7500',
        '#FF5E00',
        '#FF4700',
        '#FF0000',
      ])
      .style({
        opacity: 1,
        strokeWidth: 0,
      });
      point3Layer.on('click', (e) => {
      loading.value = true
      drawer.value = true
      info.value = {}
      title.value = point3Layer.name
      setTimeout(() => {
        info.value = e.feature
        loading.value = false
      }, 1000)
      console.log(1234, e)
    })
    const point4Layer = new PointLayer({
      name: 'Safety'
    })
      .source(data4, {
        parser: {
          type: 'json',
          x: 'longitude',
          y: 'latitude',
        },
      })
      .shape('simple')
      .size(5)
      .select({
        color: '#1677ff'
      })
      .scale('Safety', {
        // .scale('Interactivityweighted_sum', {
        type: 'linear',
        domain: [0, 1]
      })
      .color('Safety', [
        // .color('Interactivityweighted_sum', [
        '#FFFF00',
        '#FFE800',
        '#FFD100',
        '#FFBA00',
        '#FFA300',
        '#FF8C00',
        '#FF7500',
        '#FF5E00',
        '#FF4700',
        '#FF0000',
      ])
      .style({
        opacity: 1,
        strokeWidth: 0,
      });
      point4Layer.on('click', (e) => {
      loading.value = true
      drawer.value = true
      info.value = {}
      title.value = point4Layer.name
      setTimeout(() => {
        info.value = e.feature
        loading.value = false
      }, 1000)
      console.log(1234, e)
    })
    const point5Layer = new PointLayer({
      name: 'Convienance'
    })
      .source(data5, {
        parser: {
          type: 'json',
          x: 'longitude',
          y: 'latitude',
        },
      })
      .shape('simple')
      .size(5)
      .select({
        color: '#1677ff'
      })
      .scale('convenienc', {
        // .scale('Interactivityweighted_sum', {
        type: 'linear',
        domain: [0, 0.16]
      })
      .color('convenienc', [
        // .color('Interactivityweighted_sum', [
        '#FFFF00',
        '#FFE800',
        '#FFD100',
        '#FFBA00',
        '#FFA300',
        '#FF8C00',
        '#FF7500',
        '#FF5E00',
        '#FF4700',
        '#FF0000',
      ])
      .style({
        opacity: 1,
        strokeWidth: 0,
      });
      point5Layer.on('click', (e) => {
      loading.value = true
      drawer.value = true
      info.value = {}
      title.value = point5Layer.name
      setTimeout(() => {
        info.value = e.feature
        loading.value = false
      }, 1000)
      console.log(1234, e)
    })
    const point6Layer = new PointLayer({
      name: 'Sustenance'
    })
      .source(data6, {
        parser: {
          type: 'json',
          x: 'longitude',
          y: 'latitude',
        },
      })
      .shape('simple')
      .size(5)
      .select({
        color: '#1677ff'
      })
      .scale('sustenance', {
        // .scale('Interactivityweighted_sum', {
        type: 'linear',
        domain: [0, 0.16]
      })
      .color('sustenance', [
        // .color('Interactivityweighted_sum', [
       '#FFFF00',
        '#FFE800',
        '#FFD100',
        '#FFBA00',
        '#FFA300',
        '#FF8C00',
        '#FF7500',
        '#FF5E00',
        '#FF4700',
        '#FF0000',
      ])
      .style({
        opacity: 1,
        strokeWidth: 0,
      });
      point6Layer.on('click', (e) => {
      loading.value = true
      drawer.value = true
      info.value = {}
      title.value = point6Layer.name
      setTimeout(() => {
        info.value = e.feature
        loading.value = false
      }, 1000)
      console.log(1234, e)
    })
    const layerSwitch = new LayerSwitch({
      layers: [pointLayer, point6Layer, point4Layer, point5Layer, point2Layer, point3Layer],
      multiple: false
    });
    scene.addLayer(pointLayer);
    scene.addLayer(point2Layer);
    scene.addLayer(point3Layer);
    scene.addLayer(point4Layer);
    scene.addLayer(point5Layer);
    scene.addLayer(point6Layer);
    scene.addControl(layerSwitch);
  });
})
// const hide = () => {
//   hight.value = false
// }
</script>
<style>
::-webkit-scrollbar {
  display: none;
}

html,
body {
  overflow: hidden;
  margin: 0;
}

#map {
  position: absolute;
  top: 0;
  bottom: 0;
  width: 100%;
}

.info {
  width: 30%;
}

.my-content {
  max-width: 200px;
  word-wrap: break-word;
}

.tdt-bottom,
.l7-bottom {
  display: none !important;
}
</style>