<template>
<div class="searchBlock">
    <input class="searchComponents" id="searchInput" v-model="cityName" placeholder="Geb doch einfach mal ne Stadt ein... (z.B Paris)">
    <button class="searchComponents" id="searchButton" @click="getCityId">Suchen</button>

</div>
</template>

<script>
import axios from 'axios';

export default {
    name: 'searchCity',

    components: {

    },
    data: function () {
        return {
            cityName: "", // v-model Binding für Value von Input
        }
    },
    methods: {

        getCityId: function () {
            let cityId;

            axios
                .get("/api/location/search/?query=" + this.cityName) //Get-Request mit cityName
                .then(response => {
                    cityId = response.data[0].woeid; //Filtert Response und liefert cityId

                    this.$emit("cityIdFromSearch", cityId); // Übergabe der CityId an die Main App
                })
                .catch(() => {
                    alert("Diese Stadt gibts nicht... Versuchs nochmal") //Bei fehlerhafter Eingabe eine Fehlermeldung
                });
            this.cityName = ""; //Löscht die Eingabe nach der Suche
        }
    }
}
</script>

<style scoped>
.searchBlock {
    position: absolute;
    top: 25%;
    left: 30px;

    width: 80%;
    height: 10%;

    background-color: #A5B7C1;
    border: 4px solid #514644;
}

.searchComponents {
    position: relative;
    top: 35%;
    left: 17%;
}

#searchInput {
    height: 30%;
    width: 50%;
    border: 4px solid #514644;
}

#searchButton {
    margin-left: 1%;

    height: 30%;
    width: 15%;

    border: 4px solid #514644;
}
</style>
