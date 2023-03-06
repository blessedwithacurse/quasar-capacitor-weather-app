<template>
  <q-card>
    <q-card-section class="card">
      <div class="locationWrapper">
        <h5 class="location">
          {{ cityName ? cityName + ', ' + country : '' }}
        </h5>
      </div>
      <div class="tempWrapper">
        <span class="temp">{{ newTemp ? newTemp : '' }}</span>
        <div class="unitWrapper">
          <span
            :style="{
              color: activeC ? 'black' : 'gray',
            }"
            @click="changeTempUnitToCelcius"
            >{{ newTemp ? 'C&deg;' : '' }}</span
          ><span
            :style="{
              color: activeF ? 'black' : 'gray',
            }"
            class="fahrenheit"
            @click="changeTempUnitToFahrenheit"
            >{{ newTemp ? 'F' : '' }}</span
          >
        </div>
      </div>
      <p>
        {{ humidity ? $t('humidity') + Math.round(humidity) + '%' : '' }}
      </p>
      <p>
        {{
          feelsLike
            ? $t('feels_like') +
              Math.round(feelsLike) +
              'C&deg;' +
              ' | ' +
              Math.round(feelsLike ? feelsLike * (9 / 5) + 32 : 0) +
              'F'
            : ''
        }}
      </p>
      <div class="iconWrapper" v-if="weather == 'Snow'">
        {{ $t('snow') }}<span class="material-icons"> cloudy_snowing </span>
      </div>
      <div class="iconWrapper" v-else-if="weather == 'Clouds'">
        {{ $t('clouds') }}<span class="material-icons"> cloud </span>
      </div>
      <div class="iconWrapper" v-else-if="weather == 'Mist'">
        {{ $t('mist') }}<span class="material-icons"> air </span>
      </div>
      <div class="iconWrapper" v-else-if="weather == 'Clear'">
        {{ $t('clear') }}<span class="material-icons"> wb_sunny </span>
      </div>
      <div class="iconWrapper" v-else-if="weather == 'Rain'">
        {{ $t('rain') }}<span class="material-icons"> water_drop </span>
      </div>
    </q-card-section>
  </q-card>
</template>

<script lang="ts">
import { defineComponent, onMounted, onUpdated, ref } from 'vue';
export default defineComponent({
  name: 'WeatherCard',
  components: {},
  props: {
    temp: Number,
    humidity: Number,
    feelsLike: Number,
    cityName: String,
    country: String,
    weather: String,
  },
  setup(props) {
    const newTemp = ref(0);
    const activeF = ref(false);
    const activeC = ref(true);

    const changeTempUnitToFahrenheit = () => {
      newTemp.value = Math.round(props.temp ? props.temp * (9 / 5) + 32 : 0);
      activeF.value = true;
      activeC.value = false;
    };
    const changeTempUnitToCelcius = () => {
      newTemp.value = Math.round(props.temp ? props.temp : 0);
      activeC.value = true;
      activeF.value = false;
    };

    onUpdated(() => {
      newTemp.value = Math.round(props.temp ? props.temp : 0);
    });

    return {
      changeTempUnitToFahrenheit,
      newTemp,
      activeF,
      activeC,
      changeTempUnitToCelcius,
    };
  },
});
</script>

<style scoped lang="scss">
.q-card {
  margin: 16px 0px 50px 16px;
  width: 300px;
  .card {
    height: 270px;
    .locationWrapper {
      margin-bottom: 10px;
      .location {
        border-bottom: 1px solid #e6e6e6;
        margin-bottom: 10px;
        margin-top: 1px;
        padding: 0px 0px 10px 10px;
        height: 40px;
      }
    }
    .tempWrapper {
      display: flex;
      margin: 20px 0;
      .unitWrapper {
        display: flex;
        align-items: center;
        margin-left: 7px;
        font-size: 19px;
        .fahrenheit {
          margin-left: 5px;
        }
        span {
          cursor: pointer;
          color: hwb(0 41% 57% / 0.208);
          &:hover {
            color: black;
          }
        }
      }
      .temp {
        font-size: 35px;
      }
      p {
        margin-right: 50px;
      }
    }

    .iconWrapper {
      display: flex;
      align-items: center;
      span {
        margin-left: 5px;
      }
    }
  }
}
</style>
