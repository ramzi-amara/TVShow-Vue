<template>
    <div class="container" v-if="TvShow != null">
        <div class="row mb-4">
            <div class="col-5">
                <img
                    v-bind:src="TvShow.Image"
                    v-bind:alt="TvShow.Title"
                    style="max-width:450px; min-width:450px"
                />
            </div>
            <div class="col-7">
                <div class="row">
                    <div class="col-11">
                        <h2>{{TvShow.Title}}</h2>
                        <div v-if="this.$root.access_token != null" style="text-align:right" class="col-14">
                            <button type="submit" class="btn btn-success" v-on:click="AjoutFavoris" v-if="this.IsFavorite == false">
                                Add Favorite
                            </button>
                            <button type="submit" class="btn btn-danger" v-on:click="DeleteFavoris" v-if="this.IsFavorite == true">
                                Remove Favorite
                            </button>
                        </div>
                    </div>
                    <div class="col-1"></div>
                    <h5 class="col-12">{{TvShow.Year}}</h5>
                    <div class="col-3">
                        <p>{{TvShow.EpisodeCount}} Episodes</p>
                    </div>
                    <div class="col-2">
                        <p>{{TvShow.TVParentalGuideline}}</p>
                    </div>
                    <div class="col-7">
                        <p class="text-right">
                            <span
                                v-for="g in TvShow.Genres"
                                v-bind:key="g.GenreId"
                                v-bind:value="g.GenreId"
                            >{{g.Name}},</span>
                        </p>
                    </div>
                    <div class="col-2">Studio</div>
                    <div class="col-10">
                        <p>{{TvShow.Studio.Name}}</p>
                    </div>
                    <div class="col-12 text-justify">
                        <p>{{TvShow.Plot}}</p>
                    </div>
                </div>
            </div>
            <div class="row">
                <div
                    class="col-lg-2 col-md-2 col-sm-3 mb-4"
                    v-for="r in TvShow.Roles"
                    v-bind:key="r.RoleId"
                >
                    <Actor v-bind:r="r"/>
                </div>
            </div>
        </div>
        <div class="row">
            <div
                class="col-lg-3 col-md-4 col-sm-6 mb-4"
                v-for="s in TvShow.Seasons"
                v-bind:key="s.SeasonId"
            >
                <Season v-bind:s="s"/>
            </div>
        </div>
        <!--Complétez le html nécessaire pour l'affichage -->
        <!--Vous devez avoir complété la méthode GetTvShow -->
    </div>
</template>

<script>
import Actor from "@/components/Actor.vue";
import Season from "@/components/Season.vue";
export default {
    name: "DetailsTvShow",
    props: ["TvShowId"],
    components: { Actor, Season },
    data: function() {
        return {
            TvShow: null,
            IsFavorite: null
        };
    },
    methods: {
        GetTvShow: function() {
            fetch(
                `http://tvshowapi.sv55.cmaisonneuve.qc.ca/api/tvshow?TvShowId=${this.TvShowId}`
            )
                .then(response => {
                    if (!response.ok) {
                        console.log(response);
                    }
                    return response.json();
                })
                .then(data => {
                    console.log(data);
                    this.TvShow = data;
                });

            //L'objectif est d'appeller l'api /api/tvshow?TvShowId=X où X est
            //TvShowId reçu en paramètre, soit la propriété this.TvShowId
            //Placez les données reçues dans la propriété this.TvShow
            //Complétez..
            //fetch(...
        },
        CheckFavorite : function(){
            let bearerToken = 'bearer ' + this.$root.access_token;
            fetch(`${this.$root.$data.srvURL}/api/userfavorites?TvShowId=${this.TvShowId}`,{
                method : "GET",
                headers:{
                    'Authorization': bearerToken
                }
            }).then((response) => {
                if (!response.ok) { throw response; }
                return response.json();
            }).then((data) => {
                console.log(data);
                this.IsFavorite = data;
            }).catch((error) => {
                console.error(error);
            });
        },
        AjoutFavoris: function(){
            let bearerToken = 'bearer ' + this.$root.access_token;
            fetch(`${this.$root.$data.srvURL}/api/userfavorites?TvShowId=${this.TvShowId}`,{
                method : "POST",
                headers:{
                    'Authorization': bearerToken
                }
            }).then((response) => {
                if (!response.ok) { throw response; }
               
                this.IsFavorite = true;
                console.log("delete favoris:" + this.CheckFavorite)
            }).catch((error) =>{
                console.error(error);
            });   
        },
        DeleteFavoris: function(){
            let bearerToken = 'bearer ' + this.$root.access_token;
            fetch(`${this.$root.$data.srvURL}/api/userfavorites?TvShowId=${this.TvShowId}`,{
                method : "DELETE",
                headers:{
                    'Authorization': bearerToken
                }
            }).then((response) => {
                if (!response.ok) { throw response; }
               
                this.IsFavorite = false;
                 console.log("delete favoris:" + this.CheckFavorite)
            }).catch((error) =>{
                console.error(error);
            }); 
        }
    },
    mounted: function() {
        this.GetTvShow();
        this.CheckFavorite();
    }
};
</script>

<style scoped>
a {
    text-decoration: none !important;
    color: black;
}
</style>
