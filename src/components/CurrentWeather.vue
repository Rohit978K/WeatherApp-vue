<template>
  <div class="currentWeather">
    <b-card
      border-variant="light"
      :header="`${weatherInfo.location.name}, ${weatherInfo.location.country}`"
      class="text-center"
    >
      <h3>{{ weatherInfo.location.localtime }}</h3>
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
          <template #cell(precip)="data"> {{ data.item.precip }} % </template>
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
</template>
<script>
const WEATHER_KEY = "10cfd043bf34a5372e5c9b7cbb00a715";
const API_URL = `http://api.weatherstack.com/current?access_key=${WEATHER_KEY}&query=gurgaon`;
export default {
  data() {
    return {
      city: "Gurgaon",
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
      ]
    };
  },
  mounted() {
    this.fetchCurrentWeatherData();
  },
  methods: {
    async fetchCurrentWeatherData() {
      const url = `${API_URL}`;
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
          }
        ];
      });
    },
  },
};
</script>