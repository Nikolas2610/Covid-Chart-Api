<template>
  <div class="container">

    <h1 class="text-center p-5 border-bottom">Covid Stats</h1>

    <div class="chart" v-if="arrCases.length > 0">
      <div class="row mt-5">
        <!-- Title -->
        <h3 class="text-primary">Cases</h3>
        <!-- Chart -->
        <div class="col text-center">
          <BarChart :chart="arrCases" :label="'Cases'" :color="'green'" />
        </div>
      </div>

      <div class="row mt-5">
        <!-- Title -->
        <h3 class="text-primary">Hopitalized</h3>
        <!-- Chart -->
        <div class="col text-center">
          <BarChart :chart="arrHopitalized" :label="'Hopitalized'" :color="'red'" />
        </div>
      </div>

      <div class="row mt-5">
        <!-- Title -->
        <h3 class="text-primary">In Icu</h3>
        <!-- Chart -->
        <div class="col text-center">
          <BarChart :chart="arrInIcu" :label="'In Icu'" :color="'yellow'" />
        </div>
      </div>

      <div class="row mt-5">
        <!-- Title -->
        <h3 class="text-primary">On Ventilator</h3>
        <!-- Chart -->
        <div class="col text-center">
          <BarChart :chart="arrOnVentilator" :label="'On Ventilator'" :color="'blue'" />
        </div>
      </div>

      <div class="row mt-5">
        <!-- Title -->
        <h3 class="text-primary">Deaths</h3>
        <!-- Chart -->
        <div class="col text-center">
          <BarChart :chart="arrDeaths" :label="'Deaths'" :color="'black'" />
        </div>
      </div>

      <div class="row mt-5">
        <!-- Title -->
        <h3 class="text-primary">Testing</h3>
        <!-- Chart -->
        <div class="col text-center">
          <BarChart :chart="arrTesting" :label="'Testing'" :color="'orange'" />
        </div>
      </div>


    </div>

    <!-- Loader -->
    <div class="col text-center" v-else>
      <img src="./assets/images/Loading_icon.gif" alt="" srcset="">
    </div>
  </div>
</template>

<script>
import axios from 'axios';
import moment from 'moment'
import BarChart from './components/BarChart.vue'


export default {
  name: 'App',
  components: {
    BarChart
  },
  data() {
    return {
      arrCases: [],
      arrHopitalized: [],
      arrDeaths: [],
      arrInIcu: [],
      arrOnVentilator: [],
      arrTesting: [],
      chartOptions: {
        responsive: true
      }
    }
  },
  async created() {
    const { data } = await axios.get('https://api.covidtracking.com/v2/us/daily.json');

    data.data.forEach(d => {
      const date = moment(d.date, "YYYYMMDD").format('DD/MM/YYYY');
      const cases = d.cases.total.value;
      const deaths = d.outcomes.death.total.value;
      const hospitalized = d.outcomes.hospitalized.currently.value;
      const in_icu = d.outcomes.hospitalized.in_icu.currently.value;
      const on_ventilator = d.outcomes.hospitalized.on_ventilator.currently.value;
      const testing = d.testing.total.value;

      this.arrCases.push({ date, total: cases });
      this.arrHopitalized.push({ date, total: hospitalized });
      this.arrDeaths.push({ date, total: deaths });
      this.arrInIcu.push({ date, total: in_icu });
      this.arrOnVentilator.push({ date, total: on_ventilator });
      this.arrTesting.push({ date, total: testing });
    });
    console.log(this.arrCases.length > 0)
  }
}
</script>

<style>
</style>
