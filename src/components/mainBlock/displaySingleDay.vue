<template>
<div>

    <section id="displaySingleDay">

        <h1 id="date">{{cityObject.data.consolidated_weather[selectedDay].applicable_date}}</h1>

        <section class="componentPosition">
            <section class="componentInfo">

                <p>Wetter: {{cityObject.data.consolidated_weather[selectedDay].weather_state_name}}</p>

                <p>Min: {{cityObject.data.consolidated_weather[selectedDay].min_temp.toFixed(2)}} °C</p>

                <p> Max: {{cityObject.data.consolidated_weather[selectedDay].max_temp.toFixed(2)}} °C</p>

                <p> Gefühlt: {{cityObject.data.consolidated_weather[selectedDay].the_temp.toFixed(2)}} °C </p>

                <p>Vorhersage: {{cityObject.data.consolidated_weather[selectedDay].predictability}}%</p>

            </section>
        </section>

        <section class="componentPosition">
            <section class="componentInfo">
                <p>Feuchtigkeit: {{cityObject.data.consolidated_weather[selectedDay].humidity}} %</p>

                <p>Sichtbarkeit: {{(cityObject.data.consolidated_weather[selectedDay].visibility*1.609).toFixed(2)}} Km</p>

                <p>Windspeed: {{(cityObject.data.consolidated_weather[selectedDay].wind_speed*1.609).toFixed(2)}} Km/h</p>

                <p>Windrichtung: {{cityObject.data.consolidated_weather[selectedDay].wind_direction.toFixed(2)}} °</p>

                <p>Luftdruck: {{cityObject.data.consolidated_weather[selectedDay].air_pressure}} mBar</p>

            </section>
        </section>

        <section class="componentPosition">
            <img id="weatherImage" :src="'https://www.metaweather.com/static/img/weather/png/' +cityObject.data.consolidated_weather[selectedDay].weather_state_abbr+ '.png'">
        </section>

        <button id="goBackButton" @click="backButtonPressed">Zurück zur Wetteransicht</button>
    </section>

</div>
</template>

<script>
export default {
    name: 'displaySingleDay',

    props: {
        selectedDay: Number,
        cityObject: Object,
    },

    methods: {
        backButtonPressed: function () {
            this.$emit("backButtonPressed"); // Event emitet nach displayWeather
        }
    }

}
</script>

<style scoped>
#displaySingleDay {
    margin-left: auto;
    margin-right: auto;

    height: 500px;
    width: 95%;

    border: 4px solid #514644;
}

.componentPosition {
    display: inline-block;
    margin-top: -3%;
    margin-left: 2.5%;

    width: 30%;
}

.componentInfo {
    margin-left: 20%;

    font-size: 200%;
}

#weatherImage {
    margin-left: 15%;

    width: 300px;
    height: 300px;

    border: 4px solid #514644;
    background-color: white;
}

#date {
    margin-top: 1%;
    text-align: center;

    font-size: 250%;
}

#goBackButton {
    margin-left: 40%;

    height: 8%;
    width: 20%;

    font-size: 150%;

    border: 4px solid #514644;
}
</style>
