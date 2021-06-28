<template>
<div id="app">

    <h1 id="headLine">Der Wetter-Getter Yo</h1>

    <searchCity v-on:cityIdFromSearch="cityIdToDisplayWeather"></searchCity>
    <displayWeather :cityId="cityId"></displayWeather>

    <weatherExamples> </weatherExamples>

</div>
</template>

<script>
import searchCity from "./components/mainBlock/searchCity.vue";
import displayWeather from "./components/mainBlock/displayWeather.vue";

import weatherExamples from "./components/sideBar/weatherExamples.vue";

export default {
    name: 'App',
    components: {
        searchCity,
        displayWeather,

        weatherExamples,
    },
    data: function () {
        return {
            cityId: 0,
        }

    },
    methods: {

        cityIdToDisplayWeather: function (id) { //Liefert CityId aus <searchCity> zur Component <displayWeather>
            this.cityId = id;
        }
    }
}
</script>

<style>
#headLine {
    display: block;
    margin-left: auto;
    margin-right: auto;
    max-width: 40%;
    top: 6em;
    text-align: center;
    border: 4px solid #514644;
}

* {
    background-color: #DBDBE5;
}
</style>
