<template>
  <div>
    <section id="sideBlock">
      <h1 id="sideBarTitle">Wie ist das Wetter in...?</h1>

      <section v-if="dataLoaded" class="exampleCities">
        <h1 class="cityName">{{ choosenCities[0].data.title }}</h1>
        <img
          class="weatherImage"
          :src="
            'https://www.metaweather.com/static/img/weather/png/64/' +
            choosenCities[0].data.consolidated_weather[0].weather_state_abbr +
            '.png'
          "
        />

        <section class="cityTemperatures">
          <p>
            Min :
            {{
              choosenCities[0].data.consolidated_weather[0].min_temp.toFixed(2)
            }}
            °C
          </p>
          <p id="moveTemp">
            Max :
            {{
              choosenCities[0].data.consolidated_weather[0].max_temp.toFixed(2)
            }}
            °C
          </p>
        </section>
      </section>

      <section v-if="dataLoaded" class="exampleCities">
        <h1 class="cityName">{{ choosenCities[1].data.title }}</h1>
        <img
          class="weatherImage"
          :src="
            'https://www.metaweather.com/static/img/weather/png/64/' +
            choosenCities[1].data.consolidated_weather[0].weather_state_abbr +
            '.png'
          "
        />

        <section class="cityTemperatures">
          <p>
            Min :
            {{
              choosenCities[1].data.consolidated_weather[0].min_temp.toFixed(2)
            }}
            °C
          </p>
          <p id="moveTemp">
            Max :
            {{
              choosenCities[1].data.consolidated_weather[0].max_temp.toFixed(2)
            }}
            °C
          </p>
        </section>
      </section>

      <section v-if="dataLoaded" class="exampleCities">
        <h1 class="cityName">{{ choosenCities[2].data.title }}</h1>
        <img
          class="weatherImage"
          :src="
            'https://www.metaweather.com/static/img/weather/png/64/' +
            choosenCities[2].data.consolidated_weather[0].weather_state_abbr +
            '.png'
          "
        />

        <section class="cityTemperatures">
          <p>
            Min :
            {{
              choosenCities[2].data.consolidated_weather[0].min_temp.toFixed(2)
            }}
            °C
          </p>
          <p id="moveTemp">
            Max :
            {{
              choosenCities[2].data.consolidated_weather[0].max_temp.toFixed(2)
            }}
            °C
          </p>
        </section>
      </section>
    </section>
  </div>
</template>

<script>
import axios from "axios";

export default {
  name: "weatherExamples",
  components: {},
  data: function () {
    return {
      cityList: [], //Teporäre City Liste des GET Requests der Buchstaben Suche
      choosenCities: [], //3 zufallige ausgewählte Städte mit Wetterinfo

      dataLoaded: false, // //Die Daten der SideBar werden erst angezeigt, wenn Objekte geladen => dataLoaded = true
    };
  },
  async mounted() {
    var letter; // Ein zufälliger Buchstabe

    var cityNumber; //Speichert temporär eine zufällige Zahl zwischen 0 und der temporären cityList.length für eine zufällige City

    var toDisplayCitiesIds = []; // Speichert die 3 anzuzeigenden CityIds für den nächsten GET Request

    for (let f = 0; f < 3; f++) {
      letter = this.getRandomChar(); //return zufälligen Buchstaben

      await axios
        .get("/api/location/search/?query=" + letter)
        .then((response) => {
          // response ist eine CityListe mit allen Cities die den zufälligen Buchstaben enthalten
          this.cityList = response;
          cityNumber = this.getRandomCities(this.cityList); //generiert zufällige Zahl zwischen 0 und der temporären cityList.length für eine zufällige City

          toDisplayCitiesIds[f] = this.cityList.data[cityNumber].woeid; // Speichert die 3 anzuzeigenden CityIds für den nächsten GET Request
        });

      await axios
        .get("/api/location/" + toDisplayCitiesIds[f] + "/") //request für die spezifischen CityIds
        .then((response) => {
          this.choosenCities.push(response); //response wird auf choosenCities gelegt (3 Mal)

          console.log(response); //Zeigt die 3 Objekte in der Konsole
        });
    }
    this.dataLoaded = true; // Die Cities werden erst angezeigt, wenn die beiden GET Requests fertig sind
  },
  methods: {
    getRandomChar: function () {
      // generiert zufälligen Buchstaben
      var charAlphabet = new Array();
      charAlphabet = [
        "a",
        "b",
        "c",
        "d",
        "e",
        "f",
        "g",
        "h",
        "i",
        "j",
        "k",
        "l",
        "m",
        "n",
        "o",
        "p",
        "q",
        "r",
        "s",
        "t",
        "u",
        "v",
        "w",
        "x",
        "y",
        "z",
      ]; //für Random Charakter

      var randomNumber = Math.floor(Math.random() * 26); //Creates random number between 0 & 25

      let randomChar = charAlphabet[randomNumber];
      return randomChar;
    },
    getRandomCities(cityList) {
      //generiert zufällige Zahl zwischen 0 und der temporären cityList.length für eine zufällige City
      let cityIndex = Math.floor(Math.random() * cityList.data.length);

      return cityIndex;
    },
  },
};
</script>

<style>
#sideBlock {
  position: absolute;
  bottom: 30px;
  right: 30px;

  width: 15%;
  height: 90%;

  border: 4px solid #514644;
  background-color: #a5b7c1;
}

#sideBarTitle {
  text-align: center;

  font-size: 200%;

  background-color: #a5b7c1;
}

.exampleCities {
  margin: auto;
  margin-top: 3%;

  height: 250px;
  width: 250px;

  border: 4px solid #514644;
}

.cityName {
  margin-top: 5%;

  text-align: center;

  font-size: 140%;
}

.weatherImage {
  margin-left: 30%;

  height: 90px;
  width: 90px;

  border: 4px solid #514644;
  background-color: white;
}

.cityTemperatures {
  margin-top: -4%;

  text-align: center;

  font-size: 140%;
}

#moveTemp {
  margin-top: -4%;
}
</style>
