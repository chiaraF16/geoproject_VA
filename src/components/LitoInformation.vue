<template>
    <div>
            <div><b>Nome:</b> {{ this.nome }} </div>
            <div><b>Litologia:</b> {{ this.litologia }} </div>
            <div><b>Da profondità:</b> {{ this.daprof }} </div>
            <div><b>A profondità:</b> {{ this.aprof }} </div>
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
    props: ['keyData','nome', 'litologia', 'daprof', 'a prof'],
    
    components: {
        
    },
    litodata () {
        return {
            keyCoord : this.keyData,
            nomeData : this.nome,
            litogrfData : this.litologia,
            daprofData : this.daprof,
            aprofData : this.aprof,
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
            this.litologia = this.litogrfData
        },
        provincia: function(){
            this.daprof = this.daprofData
        },
        quota: function(){
            this.aprof = this.aprofData
        }, */
    }
}
</script>