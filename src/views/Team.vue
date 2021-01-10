<template>
    <div>
        
            <div class="section" v-if="team.length <= 0">
                <div class="md-layout md-alignment-center">
                    <h2>Espere un momento por favor </h2>                                            
                </div>
                <div class="md-layout md-alignment-center">                    
                    <md-progress-spinner  md-mode="indeterminate" ></md-progress-spinner>       
                </div>
            </div>              

            <div class="section" v-else>

                <div class="md-layout md-alignment-center">                    
                    <img :src="team.crestUrl" class="md-logo" />
                </div>
                <div class="md-layout md-alignment-center">                                        
                    <h1>{{team.name}}</h1>                                 
                </div>

                <div class="md-layout md-alignment-center">                                        
                    <h2>{{team.venue}}</h2>    
                </div>

                <div class="md-layout md-alignment-center">
                    
                    
                    <md-table class="md-elevation-2" v-model="squad">
                        <md-table-row>
                            
                            <md-table-head md-numeric class="md-display-2" @click="sort('position')">Posición</md-table-head>
                            <md-table-head class="md-display-2" @click="sort('name')">Nombre</md-table-head>
                            <md-table-head class="md-display-2" @click="sort('position')">Fecha Nacimiento</md-table-head>

                        </md-table-row>
                        <md-table-row v-for="player in squad" :key="player.id">                            
                            <md-table-cell md-sort-by="position">{{player.position}}</md-table-cell>
                            <md-table-cell>{{player.name}}</md-table-cell>
                            <md-table-cell>{{formatedDate(player.dateOfBirth)}}</md-table-cell>
                        </md-table-row>
                    </md-table>                    
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
        formatedDate : (date) => {
            return date.split("T")[0];
        },

        sort : (elem) => {
            this.squad.sort( (a,b) => {
                if (a[elem] > b[elem]) {
                    return 1;
                }
                if (a[elem] < b[elem]) {
                    return -1;
                }
                // a must be equal to b
                return 0;                
            });
        },

        async getTeam(){
            const instance = axios.create({
                headers : {
                    "X-Auth-Token" : '828a9e49cf6c45c7923bab4567592225',                
                },
                responseType : 'json'
            });
        
            let prodata = await instance.get(`http://api.football-data.org/v2/teams/${this.data}`)
            .then( (response) => {
                this.team = response.data;
                this.squad = this.team.squad;
                
            })
            .catch( (error) => {
                alert(error);
            });
        }
    },
    data(){
        return {
            data : '',
            team : '',    
            squad : '',                 
        }
    },
    mounted(){
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

    .md-logo{
        max-width: 150px;
        height: auto;
    }
</style>