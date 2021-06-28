<template>
<div>
    <section id="weatherBlock">

        <h1 id="cityName" v-if="dataLoaded">{{cityObject.data.title}}:</h1>

        <displaySingleDay v-if="showSingleDay" :selectedDay="selectedDay" :cityObject="cityObject" v-on:backButtonPressed="backButtonPressed"></displaySingleDay>

        <displayMultipleDays v-if="showMultipleDays" :cityObject="cityObject" v-on:selectedDay="switchView"></displayMultipleDays>

    </section>
</div>
</template>

<script>
import displaySingleDay from "./displaySingleDay.vue";
import displayMultipleDays from "./displayMultipleDays.vue";

import axios from 'axios';

export default {
    name: 'displayWeather',

    components: {
        displaySingleDay,
        displayMultipleDays,
    },

    props: {
        cityId: Number //cityId von <searchCity>
    },

    data: function () {

        return {
            cityObject: {},

            selectedDay: Number,

            dataLoaded: false, //DisplayWeather wird erst angezeigt, wenn Object geladen => dataLoaded = true

            showSingleDay: false, //Legt fest welche Ansischt angezeigt wird
            showMultipleDays: false,
        }

    },
    watch: { //Beim ändern der cityId wird automatisch die function getWeatherMultipledays aufgerufen
        cityId: function () {
            this.getWeatherMultipledays();
        }
    },

    methods: {

        getWeatherMultipledays: function () {

            axios
                .get("/api/location/" + this.cityId + "/") //Object Request mit spezifischer cityId
                .then(response => {
                    this.cityObject = response;

                    this.dataLoaded = true; // Daten geladen
                    this.showMultipleDays = true // Standartansicht wird aktiviert
                });
        },
        switchView: function (n) {

            this.showMultipleDays = false; //MultipleDays wird nicht mehr angezeigt
            this.showSingleDay = true; //SingleDay wird angezeigt

            this.selectedDay = n;
        },

        backButtonPressed: function () {
            this.showMultipleDays = true; //MultipleDays wird angezeigt
            this.showSingleDay = false; //SingleDay wird nicht mehr angezeigt
        }
    }
}
</script>

<style scoped>
#weatherBlock {
    position: absolute;
    bottom: 30px;
    left: 30px;

    width: 80%;
    height: 60%;

    border: 4px solid #514644;

}

#cityName {
    margin-left: 1.7%;

    font-size: 180%;
}

* {
    background-color: #A5B7C1;
}
</style>
