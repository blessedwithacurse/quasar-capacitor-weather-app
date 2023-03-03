<template>
  <div>
    <div class="q-pa-md">
      <div class="q-gutter-md row">
        <q-select
          filled
          @filter="fetchCities"
          :options="options.map((item: any) => (item.name + ', ' + item.country))"
          v-model="model"
          use-input
          input-debounce="0"
          :label="$t('selectCity')"
          style="width: 250px"
          behavior="menu"
          @update:model-value="(val) => fetchWeather(val)"
        >
          <template v-slot:no-option>
            <q-item>
              <q-item-section class="text-grey"> No results </q-item-section>
            </q-item>
          </template>
        </q-select>
      </div>
    </div>
    <WeatherCard
      :temp="weather.values.temp"
      :humidity="weather.values.humidity"
      :feelsLike="weather.values.feels_like"
      :cityName="city"
      :country="country"
      :weather="weatherInfo[0] ? weatherInfo[0].main : ''"
    />
  </div>
</template>

<script lang="ts">
import { defineComponent, reactive, ref } from 'vue';
import WeatherCard from './WeatherCard.vue';

const url_base_geocode = 'http://api.openweathermap.org/geo/1.0/';
const api_key = 'f370c52dafb902322d91b20d81c9442f';
const url_base = 'https://api.openweathermap.org/data/2.5/';

export default defineComponent({
  name: 'FilterSelect',
  components: {
    WeatherCard,
  },
  setup() {
    const model = ref('');
    const options = ref([]);
    const weather = reactive([]);
    const weatherInfo = ref([]);
    const country = ref('');
    const city = ref('');

    const fetchCities = (query: string, update) => {
      fetch(
        `${url_base_geocode}direct?q=${query.toLowerCase()}&limit=5&appid=${api_key}`
      ).then(async (response) => {
        const data = await response.json();
        update(() => {
          if (response.status === 200) {
            options.value = data;
          }
        });
      });
    };

    const fetchWeather = (val: string) => {
      try {
        if (val) {
          fetch(
            `${url_base}weather?q=${val.toLowerCase()}&units=metric&APPID=${api_key}`
          ).then(async (response) => {
            const data = await response.json();
            weather.values = data.main;
            country.value = data.sys.country;
            city.value = data.name;
            weatherInfo.value = data.weather;
          });
        }
      } catch (err) {
        console.log('error', err);
      }
    };

    return {
      model,
      options,
      fetchCities,
      fetchWeather,
      weather,
      country,
      city,
      weatherInfo,
    };
  },
});
</script>
