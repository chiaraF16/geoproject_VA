<template>
    <div style="position: static; padding: 10px;">
        <div style="height: 500px;position: relative; width:100%;" id="maps">
            <Graphics 
                :keyData=0
                @openGraphics="openGraphics"
            />
            <Information 
                :detailInformation="this.detailInformation"
            />

            <Maps
                :dataMaps="this.dataMaps"
                :litoMaps="this.litoMaps"
                :keyData="this.keyMaps"
                :centerCoord = "this.centerCoord"
                :detailInformation = 'this.detailInformation'
                @infoData="infoData"
            />
        </div>

        <div style="position: relative; padding:20px; width:100%;" class="graph" id="locChart"></div>
        <div style="display: flex;">
            <Table
                :dataMaps="this.dataMaps"
                @spesificData="passData"
            />
            <div style="padding:40px;width:10%;" class="graph" id="dataTable"></div>
        </div>
        <div style= "display: flex;width:100%;">
            <LitoTable
                :litoMaps="this.litoMaps"
                @spesificData="passData2"
            />
            <TempTable
                :tempMaps="this.tempMaps"
                @spesificData="passData3"
            />
        </div> 
        
         
    </div>
</template>

<script>
import Maps from './Maps.vue'
import data from '../data/pozzi3.json'
import litodata from '../data/litologia.json'
import tempdata from '../data/temperature.json'
import Table from './Table.vue'
import Information from './Information.vue'
import Graphics from './Graphics.vue'
import LitoTable from './LitoTable.vue'
import TempTable from './TempTable.vue'
import crossfilter from 'crossfilter'
//import * as d3 from 'd3'
import * as dc from 'dc';


let cf
let dPozzo
let dDaprof
let gPozzo
let gDaprof
export default {
    components: {
        Maps,
        Table,
        Information,
        Graphics,
        LitoTable,
        TempTable
    },
    data () {
        return{
            dataMaps : data, 
            litoMaps : litodata,
            tempMaps : tempdata,
            centerCoord : [],
            detailInformation: [],
            keyMaps : 0,
            keyDetail : 0,
        }
    },
    methods: {
        openGraphics(val){
          console.log(val)
          document.getElementById("mySidebarLeft").style.display = "unset";
        },
        passData(val, eye){
            this.detailInformation = []
            this.keyMaps = val
            for(var i=0; i< this.dataMaps.length; i++){
                if(this.dataMaps[i]['key'] == val){
                    this.centerCoord = this.dataMaps[i]['latLan']
                    this.detailInformation.push(this.dataMaps[i])
                    break
                }
            }
            if(eye){
                this.openNav()
            }
        },
        passData2(val, eye){
            this.detailInformation = []
            this.keyMaps = val
            for(var i=0; i< this.litoMaps.length; i++){
                if(this.dataMaps[i]['key'] == val){
                    this.centerCoord = this.dataMaps[i]['latLan']
                    this.detailInformation.push(this.dataMaps[i])
                    break
                }
            }
            if(eye){
                this.openNav()
            }
        },
        passData3(val, eye){
            this.detailInformation = []
            this.keyMaps = val
            for(var i=0; i< this.tempMaps.length; i++){
                if(this.dataMaps[i]['key'] == val){
                    this.centerCoord = this.dataMaps[i]['latLan']
                    this.detailInformation.push(this.dataMaps[i])
                    break
                }
            }
            if(eye){
                this.openNav()
            }
        },
        openNav(){
            document.getElementById("mySidebar").style.display = "unset";
        },
        creategraph(){
            this.locChart=dc.rowChart('#locChart')
                .dimension(dPozzo)
                .group(gPozzo)
                .elasticX(true)
                .data((group)=>{return group.top(12)})
            this.dataTable=dc.dataTable('#dataTable')
                .dimension(dPozzo)
                .columns(['daprof'])
                
            dc.renderAll()
        
        },
        infoData(val, eye){
            this.detailInformation = []
            this.keyMaps = val
            for(var i=0; i< this.dataMaps.length; i++){
                if(this.dataMaps[i]['key'] == val){
                    this.detailInformation.push(this.dataMaps[i])
                    break
                }
            }
            if(eye){
                this.openNav()
            }
        },
    },
    created: function(){
    },
    computed:{

    },
    mounted: function(){
        cf=crossfilter(this.litoMaps)
        dPozzo=cf.dimension(d=>d.litologia)
        dDaprof=cf.dimension(d=>d.daprof)
        gPozzo=dPozzo.group()
        gDaprof=dDaprof.group()
        this.creategraph()
        console.log(gDaprof)
    },
    watch:{
        centerCoord: function(){
            this.centerCoord
        },
        detailInformation : function(){
            this.detailInformation
        },
        keyMaps: function(){
            this.keyMaps
        },
        openNavGraphics: function(){
            this.openNavGraphics
        }
    }
}
</script>