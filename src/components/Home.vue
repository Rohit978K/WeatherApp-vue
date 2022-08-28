<template>
  <div>
    <b-row>
      <NavBar />
    </b-row>
    <b-row>
      <b-col md="4"> <CurrentWeather /></b-col>
      <b-col md="8">
        <div class="currentWeather">
          <b-card
            border-variant="primary"
            header="City Weather"
            header-bg-variant="secondary"
            header-text-variant="white"
            align="center"
          >
            <b-row>
              <b-col md="2">
                <p>{{ message }}</p></b-col
              >
              <b-col md="6">
                <b-form-select
                  id="city"
                  size="lg"
                  v-model="selected"
                  :options="options"
                  @input="fetchCityBasedWeatherData()"
                ></b-form-select>
                </b-col>
                <b-col md="4" v-if="weatherInfo.location.localtime"><p>{{weatherInfo.location.name}}, {{weatherInfo.location.country}}: {{ weatherInfo.location.localtime }}</p></b-col>
            </b-row>
            <b-row class="currentWeather">
              <b-col md="9">
                <p style="text-align: left">
                  Weather Description:
                  {{ weatherInfo.current.weather_descriptions[0] }}
                </p>
              </b-col>
              <b-col md="2">
                <img :src="imgUrl" alt="current weather" />
              </b-col>
            </b-row>
            <b-row class="mt-2">
              <b-table stacked show-empty :items="items" :fields="fields">
                <template #cell(temperature)="data">
                  {{ data.item.temperature }} °C
                </template>
                <template #cell(feelslike)="data">
                  {{ data.item.feelslike }} °C
                </template>
                <template #cell(wind_dir)="data">
                  {{ data.item.wind_dir }} {{ data.item.wind_speed }} km/h
                </template>
                <template #cell(pressure)="data">
                  {{ data.item.pressure }} mb
                </template>
                <template #cell(precip)="data">
                  {{ data.item.precip }} %
                </template>
                <template #cell(humidity)="data">
                  {{ data.item.humidity }} %
                </template>
                <template #cell(uv_index)="data">
                  {{ data.item.uv_index }}
                </template>
                <template #cell(visibility)="data">
                  {{ data.item.visibility }} km
                </template>
              </b-table>
            </b-row>
          </b-card>
        </div>
      </b-col>
    </b-row>
  </div>
</template>

<script>
import CurrentWeather from "./CurrentWeather.vue";
import NavBar from "./NavBar.vue";
import STRINGS from "../utility/string.utility";
import URL_UTILITY from "../utility/url.utility";
export default {
  name: "Home",
  components: {
    CurrentWeather,
    NavBar,
  },
  data() {
    return {
      message: STRINGS.message,
      imgUrl: "",
      weatherInfo: {
        current: {
          weather_descriptions: [""],
        },
        location: {
          name: '',
          country: '',
          localtime: ''
        }
      },
      selected: null,
      options: STRINGS.cityList,
      items: [],
      fields: [
        "temperature",
        { key: "feelslike", label: "Real Feel" },
        "wind_speed",
        "pressure",
        { key: "precip", label: "Precipitation" },
        "humidity",
        { key: "uv_index", label: "UV Index" },
        "visibility",
      ],
    };
  },
  methods: {
    async fetchCityBasedWeatherData() { // to get city based
      if(!this.selected) {
        this.weatherInfo = {
        current: {
          weather_descriptions: [""],
        },
        location: {
          name: '',
          country: '',
          localtime: ''
        }
      };
      this.imgUrl = "";
      this.items = [];
        return;
      }
      const url = `${URL_UTILITY.getWeatherByCityNameUrl}?access_key=${STRINGS.WEATHER_KEY}&query=${this.selected}`;
      const response = await fetch(url);
      const data = response.json();
      data.then((key) => {
        this.weatherInfo = key;
        this.imgUrl = key.current.weather_icons;
        this.items = [
          {
            temperature: key.current.temperature,
            feelslike: key.current.feelslike,
            wind_dir: key.current.wind_dir,
            wind_speed: key.current.wind_speed,
            pressure: key.current.pressure,
            precip: key.current.precip,
            humidity: key.current.humidity,
            precip: key.current.precip,
            uv_index: key.current.uv_index,
            visibility: key.current.visibility,
          },
        ];
      });
    },
  },
};
</script>
<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h1,
h2 {
  font-weight: normal;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
</style>
