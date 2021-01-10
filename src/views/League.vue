<template>
    <div class="wrapper">
        <div class="main ">
            <div class="section" v-if="league.length <= 0">
                <div class="md-layout md-gutter md-alignment-center">
                    <h2>Espere un momento por favor </h2>                                            
                </div>
                <div class="md-layout md-gutter md-alignment-center">                    
                    <md-progress-spinner  md-mode="indeterminate" ></md-progress-spinner>       
                </div>
            </div>              

            <div class="section" v-else>
                <div class="md-layout md-gutter md-alignment-center">
                    <h1>{{competition_data.name}}</h1> 
                </div>


                <div class="md-layout md-gutter md-alignment-center">
                    
                    
                    <md-table class="md-elevation-2">
                        <md-table-row>
                            <md-table-head md-numeric>Posición</md-table-head>
                            <md-table-head>Equipo</md-table-head>
                            <md-table-head>Puntos</md-table-head>
                            <md-table-head>PJ</md-table-head>
                            <md-table-head>GF</md-table-head>
                            <md-table-head>GC</md-table-head>
                        </md-table-row>
                        <md-table-row v-for="(team,index) in league" :key="index">
                            <md-table-cell md-numeric>{{team.position}}</md-table-cell>
                            <md-table-cell>
                                    <img class="logo" :src="team.team.crestUrl" :alt="team.team.name">
                                <router-link :to="'/team/'+team.team.id">{{team.team.name}}</router-link>
                            </md-table-cell>                            
                            <md-table-cell>{{team.points}}</md-table-cell>
                            <md-table-cell>{{team.playedGames}}</md-table-cell>
                            <md-table-cell>{{team.goalsFor}}</md-table-cell>
                            <md-table-cell>{{team.goalsAgainst}}</md-table-cell>
                        </md-table-row>
                    </md-table>                    
                </div>
            </div>          

        
        </div>
    </div>
</template>

<script>
import axios from 'axios';
export default {

    name : 'league',
    metaInfo: {
      title: 'Liga',
      titleTemplate: '%s - FutiApp',
    },

    methods : {
        async getTeam(){
            const instance = axios.create({
                headers : {
                    "X-Auth-Token" : '828a9e49cf6c45c7923bab4567592225',                
                },
                responseType : 'json'
            });
        
            let prodata = await instance.get(`http://api.football-data.org/v2/competitions/${this.data}/standings`)
            .then( (response) => {
                this.competition_data = response.data.competition;
                this.league = response.data.standings[0].table;
            })
            .catch( (error) => {
                alert(error);
            });
        }
    },
    computed(){
            
    },
    data(){
        return {
            data : '',
            league : [],
            competition_data : ''           
        }
    },
    created(){
        this.data = this.$route.params.id;
        this.getTeam();
     
    }
}
</script>

<style>
    .no-bullets{
        list-style-type: none;
    }
    .logo{
        max-width: 75px;
        height: auto;
    }
</style>