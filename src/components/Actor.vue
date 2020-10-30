<template>
    <div>
        <div class="card" style="min-width: 160px; max-width: 160px;">
            <img
                v-bind:src="r.Image"
                alt="r.Character"
                class="card-img-top mx-auto rounded role-img"
                style="min-width: 160px; max-width: 160px; min-height: 240px; max-height: 240px;"
            />
        </div>
        <div class="card-body text-center" style="min-height: 140px;">
            <p class="card-title small" style="font-weight: bold;">{{r.Name}}</p>
            <p class="small">{{r.Character}}</p>
        </div>
    </div>
</template>

<script>
export default {
    name: "Actor",
    props: { r: Object }
};
</script>

<style>
</style>