<template>
    <div>
        <div class="sidebarInformation" id="mySidebar" >
            <a href="javascript:void(0)" class="closebtn" @click="closeNav">&times;</a>
            <div><b>Nome:</b> {{  this.detail.nome }} </div>
            <div><b>Locazione:</b> {{ this.detail.entitam }} </div>
            <div><b>Provincia:</b> {{ this.detail.provincia }} </div>
            <div><b>Regione:</b> {{ this.detail.regione }} </div>
            <div><b>Geothermal Field:</b> {{ this.detail.camploc }} </div>
            
            <br/>
            <h5>Informazioni sul pozzo</h5>
            <div><b>Tipo:</b> {{ this.detail.tipo }} </div>
            <div><b>Quota:</b> {{ this.detail.quota }} </div>
            <div><b>Max profondità:</b> {{ this.detail.prof }} </div>
            <div><b>Esito:</b> {{ this.detail.esito }} </div>
            <div><b>Scopo:</b> {{ this.detail.scopo }} </div>
            <div><b>Uso:</b> {{ this.detail.uso }} </div>
            <div><b>Posizione:</b> {{ this.detail.posizione }} </div>
            <div><b>Stato:</b> {{ this.detail.stato }} </div>
            <div><b>Proprietario:</b> {{ this.detail.proprietar }} </div>
        
        </div>
    </div>
</template>

<script>


export default {
    props: ['detailInformation'],
    data () {
        return{
            detail : {
                nome : null,
                entitam : null,
                provincia : null,
                quota : null,
                camploc : null,
                regione : null,
                tipo : null,
                proprietar : null, 
                datacomp : null,
                prof : null,
                esito : null,
                scopo : null,
                uso : null,
                posizione : null, 
                stato : null,
            },
        }
    },
    methods: {
        closeNav(){
            document.getElementById("mySidebar").style.display = "none";
        },
    },
    created: function(){

    },
    computed:{

    },
    mounted: function(){
    },
    watch:{
        detail: function(){

        },
        detailInformation: function(){
            if(this.detailInformation[0] != null){
                this.detail.nome = this.detailInformation[0].nome
                this.detail.entitam = this.detailInformation[0].entitam
                this.detail.provincia = this.detailInformation[0].provincia
                this.detail.quota = this.detailInformation[0].quota
                this.detail.camploc = this.detailInformation[0].camploc
                this.detail.regione = this.detailInformation[0].regione
                this.detail.tipo = this.detailInformation[0].tipo
                this.detail.proprietar = this.detailInformation[0].proprietar
                this.detail.datacomp = this.detailInformation[0].datacomp
                this.detail.prof = this.detailInformation[0].prof
                this.detail.esito = this.detailInformation[0].esito
                this.detail.scopo = this.detailInformation[0].scopo
                this.detail.uso = this.detailInformation[0].uso
                this.detail.posizione = this.detailInformation[0].posizione
                this.detail.stato = this.detailInformation[0].stato
            }
        }


    }
}
</script>
<style>
.sidebarInformation{
    position: absolute;
    background-color:coral;
    float: right;
    right: 0;
    height: 500px;
    transition: 0.5s;
    width: 20%;
    top: 0;
    z-index : 1;
    display: none;
    font-size:12px;
    padding: 30px;
    padding-top: 55px;
}

.sidebarInformation .closebtn {
  position: absolute;
  top: 0;
  font-size: 36px;
  padding-right:30px;
  padding-top:25px;
}

</style>