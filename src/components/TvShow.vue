<template>
 <div class="col-lg-3 col-md-4 col-sm-6 col-12 mb-4">
    <router-link :to="{ name: 'DetailsTvShow', params: { TvShowId: t.TvShowId } }">
        <div class="card rounded">
            <img class="card-img-top" v-bind:src="t.Image" />
            <div class="card-body text-center" style="max-heigth:210px;">
                <h4 class="card-title" style="min-height:50px;max-height:50px;">{{t.Title}}</h4>
                <p v-if="t.SeasonCount == 1">season {{t.SeasonCount}}</p>
                <div></div>
                <p v-if="t.SeasonCount > 1">season {{t.SeasonCount}}</p>
                <div></div>
            </div>
        </div>
    </router-link>
 </div>
</template>

<script>
export default {
    name: "TvShow",
    props: { t: Object }
};
</script>

<style>
</style>