<template>
<div>
    <section id="displayDay" v-for="day in days" v-bind:key="day" @click="selectedDay(day)">

        <h1 id="date">{{cityObject.data.consolidated_weather[day-1].applicable_date}}</h1>

        <img id="weatherImage" :src="'https://www.metaweather.com/static/img/weather/png/' +cityObject.data.consolidated_weather[day-1].weather_state_abbr+ '.png'">
        <section id="weatherInfo">
            <p>
                Wetter:{{cityObject.data.consolidated_weather[day-1].weather_state_name}}
                <p>
                    Min. :{{cityObject.data.consolidated_weather[day-1].min_temp.toFixed(2)}} °C <br>
                    Max. :{{cityObject.data.consolidated_weather[day-1].max_temp.toFixed(2)}} °C<br>
                    Gefühlt :{{cityObject.data.consolidated_weather[day-1].the_temp.toFixed(2)}} °C
                </p>

                <p>
                    Vorhersage:{{cityObject.data.consolidated_weather[day-1].predictability}}%
                </p>
        </section>

    </section>

</div>
</template>

<script>
export default {
    name: 'displayMultipleDays',

    data: function () {
        return {
            days: 5 //Schleife um 5 tage anzuzeigen 
        }
    },
    props: {
        cityObject: {},
    },

    methods: {

        selectedDay: function (day) {

            this.$emit("selectedDay", day - 1); // n-1 wegen Arrayspeicherung && Übergibt den ausgewählten Tag an DisplayWeather
        }
    }
}
</script>

<style scoped>
#displayDay {
    float: left;
    margin-left: 1.7%;
    margin-top: 1.5%;

    height: 450px;
    width: 300px;

    border: 4px solid #514644;
}

#date {
    text-align: center;
    font-size: 180%;

}

#weatherImage {
    margin-left: 25%;

    height: 150px;
    width: 150px;

    border: 4px solid #514644;
    background-color: white;
}

#weatherInfo {
    text-align: center;
    font-size: 125%;
}
</style>
