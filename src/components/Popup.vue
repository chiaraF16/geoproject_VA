<template>
    <div>
            <div><b>Nome:</b> {{ this.nome }} </div>
            <div><b>Provincia:</b> {{ this.provincia }} </div>
            <div><b>Quota:</b> {{ this.quota }} </div>
            <div><b>Data:</b> {{ this.datacomp }} </div>
            <div>
                <button type="button" class="btn btn-outline-primary btn-sm" style="float:right;" @click="openNav()">
                    <i class="fa fa-eye"></i>
                </button>
            </div>
            <br/>
            <br/>
    </div>
</template>

<script>
export default {
    props: ['keyData','nome', 'locgeo', 'provincia', 'quota', 'datacomp'],
    
    components: {
        
    },
    data () {
        return {
            keyCoord : this.keyData,
            nomeData : this.nome,
            locgeoData : this.locgeo,
            provinciaData : this.provincia,
            quotaData : this.quota,
            datacompData : this.datacomp,
        }
    },
    methods: {
        visibleClass() {
            this.visible = !this.visible
            console.log(this.visible) 
        },
        openNav(){
            this.$emit('infoData', this.keyCoord, true)
            /*document.getElementById("mySidebar").style.display = "unset";*/
        }
    },
    created: function(){

    },
    computed:{

    },
    mounted: function(){

    },
    watch:{
        keyCoord: function(){
            this.keyData = this.keyCoord
        },         /*
        nome: function(){
            this.nome = this.nomeData
            console.log(this.nome)
        },

        locgeo: function(){
            this.locgeo = this.locgeoData
        },
        provincia: function(){
            this.provincia = this.provinciaData
        },
        quota: function(){
            this.quota = this.quotaData
        },
        datacomp: function(){
            this.datacomp = this.datacompData
        }, */
    }
}
</script>