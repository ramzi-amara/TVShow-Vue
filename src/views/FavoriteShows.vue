<template>
    <div class="container" v-if="TvShows!=null">
        <div class="row">
            <H1 v-if="TvShows.length == 0">You got no favorite</H1>
            <TvShow v-for="t in TvShows" v-bind:key="t.TvShowId" v-bind:t="t" />
        </div>
    </div>
</template>

<script>
    import TvShow from '@/components/TvShow.vue'
    export default {
        name: "FavoriteShows",
        components:{TvShow},
        data : function(){
            return{
                TvShows: null
            };
        },
        methods :{
            getFavoriteShow : function(){
                let bearerToken = 'bearer ' + this.$root.access_token;
                fetch(`${this.$root.$data.srvURL}/api/favoriteTvShows`, {
                    method : "GET",
                    headers:{
                        'Authorization': bearerToken
                    }
                }).then((response) => {
                    if (!response.ok) { throw response; }
                    return response.json();
                }).then((data) => {
                    console.log(data);
                    this.TvShows = data;
                }).catch((error) => {
                    console.error(error);
                });
            },
        },
        mounted : function(){
            this.getFavoriteShow();
        }
    };
</script>

<style></style>
