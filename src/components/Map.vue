<template>
    <div style="height:600px" class="container-fluid">
        <div id="mapview" class="map">
        </div>
    </div>
    <button @click="createVectorMarker({ id: '1', x: 114, y: 30 })">createLabel</button>
    <button @click="deleteVectorMarker(1)">deleteVectorMarker</button>
</template>
<script>
import 'ol/ol.css'
import { Map, View, Feature } from 'ol'
import OSM from 'ol/source/OSM'
import * as olProj from 'ol/proj'
import VectorSource from 'ol/source/Vector'
import geojson from "@/assets/geojson.json";
import TileLayer from 'ol/layer/Tile'
import VectorLayer from 'ol/layer/Vector';

export default {
    name: 'MyMapView',
    data: function () {
        return {
            map: null,
            iconLayer: null
        }
    },
    methods: {
        //  初始化地图
        initMap: function () {
            this.map = new Map({
                target: 'mapview',
                layers: [
                    new TileLayer({
                        source: new ol.source.OSM()
                    }),
                    new VectorLayer({
                        source: new VectorSource({
                            features: (new ol.format.GeoJSON({ featureProjection: 'EPSG:3857' })).readFeatures(geojson)
                        })
                    })
                ],
                view: new View({
                    center: olProj.fromLonLat([114.224486, 30.654155]),
                    zoom: 6
                })
            });
        },

        // 主要思想构建Feature，构建Source，构建Layer，最后添加Layer到map即可
        // 相关变量在data中申明即可，eg：iconLayer: null,
        // 添加图标
        // 核心思想和加图标图片一样，构建Feature，构建Source，构建Layer，最后添加Layer到map即可
        // 加点
        createVectorMarker(obj) {
            // 创建一个唯一的标识符
            var uniqueId = obj.id;

            // 将经度和纬度转换为 OpenLayers 的坐标对象
            var lonlat = ol.proj.fromLonLat([obj.x, obj.y]);

            // 创建一个矢量标注样式
            var style = new ol.style.Style({
                image: new ol.style.RegularShape({
                    fill: new ol.style.Fill({ color: 'blue' }),
                    stroke: new ol.style.Stroke({ color: 'white', width: 2 }),
                    points: 4,
                    radius: 10,
                    angle: Math.PI / 4
                })
            });

            // 创建一个矢量要素
            var feature = new ol.Feature({
                geometry: new ol.geom.Point(lonlat),
                name: obj.id
            });
            feature.setId(uniqueId);

            // 将样式应用于要素
            feature.setStyle(style);

            // 创建一个矢量标注图层
            this.iconLayer = new ol.layer.Vector({
                source: new ol.source.Vector({
                    features: [feature]
                })
            });

            // 将矢量标注图层添加到地图中
            this.map.addLayer(this.iconLayer);
        },
        deleteVectorMarker(id) {
            var layers = this.map.getLayers().getArray();
            var layerToRemove = null;
            console.log(layers.length);
            // for (var i = 0; i < layers.length; i++) {

            //     if (this.iconLayer.source.feature.name == id) {
            //         layerToRemove = layers[i];
            //         break;
            //     }
            // }
            // if (layerToRemove) {
            //     this.map.removeLayer(layerToRemove);
            // }
        }
    },
    mounted() {
        this.initMap()
    }
}
</script>
    
<style scoped>
#mapview {
    height: 100%;
    width: 70%;
    flex: 1;
    border: 1px #ccc solid;
}
</style>
    
    