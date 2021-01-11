<template>
    <div class="wrapper">
        <div class="main">
            <div class="section text-center">
                <h1>FutiApp</h1>
                <h2>Una pequeña app para seguir las ligas de futbol en Europa :D </h2>
                <h3>Selecciona un país y conoce las posiciones en sus ligas</h3>
            </div>
            <div class="section">
                
                <div class="md-layout md-gutter md-alignment-center">
                    <div class="px-5 md-layout-item md-medium-size-33 md-small-size-50 md-xsmall-size-100" v-for="country in countries" :key="country.id">
                                            
                        <router-link :to="'/league/'+country.id">
                            <img alt="pais"  :src="link+country.prefix+'.svg'">
                            <h6>{{country.name}}</h6>
                        </router-link>                    
                    </div>
                </div>

            </div>
        </div>
    </div>

    
        
            


    
</template>

<script>
export default {
    metaInfo: {
      title: 'FutiApp',
      titleTemplate: '%s - FutiApp',
    },    
    data(){
        return {

            countries : [
                { prefix : "GB", id : "2021", name : "Inglaterra"},
                {prefix : "IT",id : "2019", name : "Italia"},
                {prefix : "ES",id : "2014", name : "España"},
                {prefix : "FR",id : "2015", name : "Francia"},
                {prefix : "DE",id : "2002", name : "Alemania"}
            ],
            link : "http://purecatamphetamine.github.io/country-flag-icons/3x2/"
        }
    }
}
</script>

<style>

</style>