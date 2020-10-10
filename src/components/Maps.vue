<template>

  <div style="height: 500px;position:sticky;">
    <l-map name="map"
      style="height: 100%; width: 100%"
      :zoom="zoom"
      :center="center"
      @update:zoom="zoomUpdated"
      @update:center="centerUpdated"
      @update:bounds="boundsUpdated"
      :options="{zoomControl: false, /*scrollWheelZoom:false */ }"
    >
      
      <l-control-scale position="bottomleft" :imperial="true" :metric="true"></l-control-scale>
      <l-control-zoom position="bottomright"  ></l-control-zoom>
      <l-control-layers position="topright"></l-control-layers>
      <l-circle-marker
          v-for="circle in this.dataMaps" 
          :key=circle.key
          :lat-lng=circle.latLan
          :radius=calculateRadius(circle.quota)
          :color=circle_setting.color
          :fill=circle_setting.fill
          :weight=circle_setting.weight
          :fillOpacity=circle_setting.fillOpacity
          :opacity=circle_setting.opacity
          :fillColor='circle_setting.fillColor'
      ><l-popup> 
            <Popup 
                :keyData="circle.key"
                :nome="circle.nome" 
                :locgeo ="circle.locgeo"
                :provincia="circle.provincia"
                :quota="circle.quota"
                :datacomp="circle.datacomp"
                @infoData="infoData"
            />
        </l-popup>
      </l-circle-marker>
      <l-circle-marker name="selected_circle"
          :key="selectedCircle_setting.key"
          :lat-lng="selectedCircle_setting.latLan"
          :radius=selectedCircle_setting.radius
          :color=selectedCircle_setting.color
          :fill=selectedCircle_setting.fill
          :weight=selectedCircle_setting.weight
          :fillOpacity=selectedCircle_setting.fillOpacity
          :opacity=selectedCircle_setting.opacity
          :fillColor='selectedCircle_setting.fillColor'
      >
        <l-popup>
            <Popup 
                :keyData="this.keyData"
                :nome="this.selectedCircle_setting.nome" 
                :locgeo="this.selectedCircle_setting.locgeo"
                :provincia="this.selectedCircle_setting.provincia"
                :quota="this.selectedCircle_setting.quota"
                :datacomp="this.selectedCircle_setting.datacomp"
                @infoData="infoData"
            />
        </l-popup>
      </l-circle-marker>
      <div >
      <l-tile-layer id="map"
            v-for="tileProvider in tileProviders"
            :key="tileProvider.name"
            :name="tileProvider.name"
            :visible="tileProvider.visible"
            :url="tileProvider.url"
            :attribution="tileProvider.attribution"
            layer-type="base"
            />
      <l-control position="topleft">
        <button type="button" class="btn btn-light btn-md" @click="openGraphics()">
            <i class="fa fa-bar-chart"></i>
        </button>
      </l-control>
      </div>

    </l-map>
  </div>
</template>

<script>
import {LMap, LTileLayer, LControlLayers, LCircleMarker, LControlZoom, LControlScale, LPopup, LControl} from 'vue2-leaflet';
import 'leaflet/dist/leaflet.css';
import Popup from './Popup.vue';


export default {
  props: ['dataMaps', 'keyData', 'centerCoord', 'detailInformation'],
  components: {
    LMap,
    LTileLayer,
    LControlLayers,
    LCircleMarker,
    LControlZoom,
    LControlScale,
    LPopup,
    Popup,
    LControl,
  },
  data () {
    return {
      detail : null,
      key : this.keyData,
      zoom: 5.5,
      center: [41.447147, 14.567837],
      changedCenter : false,
      circle_setting : 
        {
          fill : true,
          fillColor: 'red',
          radius: 4,
          color : 'black',
          weight: 0.3,
          opacity: 1.0,
          fillOpacity : 1.0,
        },
      selectedCircle_setting : 
        {
          key : null,
          latLan : [0,0],
          fill : true,
          fillColor: 'red',
          radius: 15,
          color : 'black',
          weight: 1,
          opacity: 1.0,
          fillOpacity : 1.0,
          visible: false,
          nome : null,
          provincia : null,
          locgeo : null,
          quota : null,
          datacomp : null,
        },
      bounds: null,
      tileProviders: [
        {
          name: 'Toner Map',
          visible: false,
          url: 'http://{s}.tile.stamen.com/toner/{z}/{x}/{y}.png',
          attribution:
            'Map tiles by <a href="http://stamen.com">Stamen Design</a>, under <a href="http://creativecommons.org/licenses/by/3.0">CC BY 3.0</a>. Data by <a href="http://openstreetmap.org">OpenStreetMap</a>, under <a href="http://www.openstreetmap.org/copyright">ODbL</a>.',
        },
        {
          name: 'POIs Map',
          visible: false,
          attribution:
            '&copy; <a target="_blank" href="http://osm.org/copyright">OpenStreetMap</a> contributors',
          url: 'https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png',
        },
        {
          name: 'Topography Map',
          visible: true,
          url: 'https://{s}.tile.opentopomap.org/{z}/{x}/{y}.png',
          attribution:
            'Map data: &copy; <a href="http://www.openstreetmap.org/copyright">OpenStreetMap</a>, <a href="http://viewfinderpanoramas.org">SRTM</a> | Map style: &copy; <a href="https://opentopomap.org">OpenTopoMap</a> (<a href="https://creativecommons.org/licenses/by-sa/3.0/">CC-BY-SA</a>)',
        },
        {
          name: 'Terrain Map',
          visible: false,
          url: 'http://{s}.tile.stamen.com/terrain/{z}/{x}/{y}.png',
          attribution:
            'Map tiles by <a href="http://stamen.com">Stamen Design</a>, under <a href="http://creativecommons.org/licenses/by/3.0">CC BY 3.0</a>. Data by <a href="http://openstreetmap.org">OpenStreetMap</a>, under <a href="http://www.openstreetmap.org/copyright">ODbL</a>',
        },
      ],
    };
  },
  methods: {
    zoomUpdated (zoom) {
      if(this.changedCenter){
        this.zoom = 15
        this.changedCenter = false
      }
      else{
        this.zoom = zoom;
      }
    },
    centerUpdated (center) {
      this.center = center;
    },
    boundsUpdated (bounds) {
      this.bounds = bounds;
    },
    infoData(val, eye){
      this.$emit('infoData', val, eye)
    },
    calculateRadius(val){
      return parseFloat(val)/100
    },
    openGraphics(){
      document.getElementById("mySidebarLeft").style.display = "unset";
    }

  },
  computed:{

  },
  
  mounted(){
    
  },
  created(){
  },
  watch:{
    centerCoord : function(){
      this.changedCenter = true
      this.detail = this.detailInformation
      this.selectedCircle_setting.key = this.key
      this.selectedCircle_setting.nome = this.detailInformation[0].nome
      this.selectedCircle_setting.locgeo = this.detailInformation[0].locgeo
      this.selectedCircle_setting.provincia = this.detailInformation[0].provincia
      this.selectedCircle_setting.quota = this.detailInformation[0].quota
      this.selectedCircle_setting.datacomp = this.detailInformation[0].datacomp
      this.selectedCircle_setting.latLan = this.centerCoord
      this.selectedCircle_setting.visible = true
      this.centerUpdated(this.centerCoord)
    },
    keyData : function(){
      this.keyData
    },
    selectedCircle_setting: function(){
      this.selectedCircle_setting
    }
  }
}
</script>
