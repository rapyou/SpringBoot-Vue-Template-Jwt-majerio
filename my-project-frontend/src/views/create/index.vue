<template>
    <el-container class="thin ful-fill">
        <el-aside width="420px" class="thin">
            <el-steps style="max-width: 610px;margin: 35px 30px 35px 130px;height:85%" :active="1" direction="vertical">
                <el-step title="算法" :icon="Operation" />
                <el-step title="选择影像" :icon="Picture" />
                <el-step title="计算范围" :icon="Place" />
            </el-steps>
        </el-aside>
        <el-main class="thin">
            <el-container class="full-fill">
                <el-main class="thin">
                    <el-form :model="form" label-width="auto" style="max-width: 600px">
                        <el-form-item label="算法">

                            <el-select v-model="algorithm" clearable placeholder="请选择算法" style="width: 340px">
                                <el-option v-for="item in algorithmList" :key="item.value" :label="item.value"
                                    :value="item.value" />
                            </el-select>
                        </el-form-item>
                        <el-form-item label="影像">

                            <el-select v-model="image" clearable placeholder="请选择影像" style="width: 340px">
                                <el-option v-for="item in imageList" :key="item.value" :label="item.value"
                                    :value="item.value" />
                            </el-select>
                        </el-form-item>

                    </el-form>
                    <div id="create-task-map">

                    </div>
                </el-main>
                <el-footer height="50px"
                    style="background-color: aliceblue;margin:20px;border:1px solid #cccccc"></el-footer>
            </el-container>
        </el-main>
    </el-container>
</template>

<script setup>
import { ref, onMounted } from 'vue'
import { Operation, Picture, Place } from '@element-plus/icons-vue'
import L from "leaflet";
import 'leaflet-switch-basemap/src/L.switchBasemap'
// import config from "../config";
import 'leaflet-switch-basemap/src/L.switchBasemap.css'
let map = null;
const algorithm = ref('')
const image = ref('')
const algorithmList = [{
    value: 'NDVI'
}, {
    value: 'EVI'
}, {
    value: 'LAI'
}]
const imageList = [{
    value: '20180612'
}, {
    value: '20180719'
}, {
    value: '20180924'
}, {
    value: '20181002'
}]
onMounted(() => {

    const files = [
        "./lib/geotiff/geotiff.js",
        "./lib/geotiff/plotty.js",
        "./lib/geotiff/leaflet-geotiff.js",
        "./lib/geotiff/leaflet-geotiff-plotty.js",
        "./lib/geotiff/leaflet-geotiff-vector-arrows.js",
    ];
    loadScripts(files, function () {
        console.log("All scripts loaded");
        initMap();
    });
})

const loadScripts = (files, callback) => {
  if (files.length === 0) {
    callback();
    return;
  }
  const file = files.shift();
  const script = document.createElement("script");
  script.onload = function () {
    loadScripts(files, callback);
  };
  script.src = file;
  document.head.appendChild(script);
};
const initMap = () => {
    map = L.map("create-task-map", {
        attributionControl: false,
    }).setView([32.336, 118.22513], 16);
    new L.basemapsSwitcher([
        {
            layer: L.tileLayer('http://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png', {
                attribution: '&copy; <a href="https://www.openstreetmap.org/copyright">OpenStreetMap</a> contributors'
            }).addTo(map), //DEFAULT MAP
            icon: './lib/images/1.png',
            name: 'osm'
        },
        {
            layer: L.tileLayer('https://tiles.stadiamaps.com/tiles/alidade_smooth/{z}/{x}/{y}{r}.png', {
                attribution: '&copy; <a href="https://stadiamaps.com/">Stadia Maps</a>, &copy; <a href="https://openmaptiles.org/">OpenMapTiles</a> &copy; <a href="http://openstreetmap.org">OpenStreetMap</a> contributors'
            }),
            icon: './lib/images/2.png',
            name: 'alidade'
        },
        {
            layer: L.tileLayer('https://{s}.tile.opentopomap.org/{z}/{x}/{y}.png', {
                attribution: 'Map data: &copy; <a href="https://www.openstreetmap.org/copyright">OpenStreetMap</a> contributors, <a href="http://viewfinderpanoramas.org">SRTM</a> | Map style: &copy; <a href="https://opentopomap.org">OpenTopoMap</a> (<a href="https://creativecommons.org/licenses/by-sa/3.0/">CC-BY-SA</a>)'
            }),
            icon: './lib/images/3.png',
            name: 'opentopo'
        },
    ], { position: 'topright' }).addTo(map);
    
    var layer = L.tileLayer.wms('http://localhost:8080/geoserver/wms', {
        layers: 'yy:gd',
        format: 'image/png',
        transparent: true,
        version: '1.1.1'
    }).addTo(map);
}
</script>
<style scoped>
#create-task-map {
    height: 100%;
    width: 90%;
    background-color: aliceblue;
}
</style>
