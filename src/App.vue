<template>
  <div id="app" class="container">

    <div class="row mt-5">

      <div class="col">
        <h3 class="text-center">
          COVID-19 STATISTICS</h3>
      </div>

    </div>

    <div class="row mt-5" v-if="arrPositive.length > 0">

      <div class="col">
        <h4 class="text-center">positive.</h4>
        <line-chart
          :chartData="arrPositive"
          :options="chartOptions"
          label="Positive"
          :chartColors="positiveChartColors">
        </line-chart>
      </div>
    </div>

    <div class="row mt-5" v-if="arrHospitalized.length > 0">

      <div class="col">
        <h4 class="text-center">hospitalized.</h4>
        <line-chart
          :chartData="arrHospitalized"
          :options="chartOptions"
          label="Hospitalized"
          :chartColors="hospitalizedChartColors">
        </line-chart>
      </div>
    </div>

    <div class="row mt-5" v-if="arrInIcu.length > 0">

      <div class="col">
        <h4 class="text-center">in ICU.</h4>
        <line-chart
          :chartData="arrInIcu"
          :options="chartOptions"
          label="In ICU"
          :chartColors="inIcuColors">
        </line-chart>
      </div>
    </div>

    <div class="row mt-5" v-if="arrOnVentilators.length > 0">

      <div class="col">
        <h4 class="text-center">on ventilators.</h4>
        <line-chart
          :chartData="arrOnVentilators"
          :options="chartOptions"
          label="On Ventilators"
          :chartColors="onVentilatorsColors">
        </line-chart>
      </div>
    </div>

    <div class="row mt-5" v-if="arrRecovered.length > 0">

      <div class="col">
        <h4 class="text-center">recovered.</h4>
        <line-chart
          :chartData="arrRecovered"
          :options="chartOptions"
          label="Recovered"
          :chartColors="recoveredColors">
        </line-chart>
      </div>
    </div>

    <div class="row mt-5" v-if="arrDeaths.length > 0">

      <div class="col">
        <h4 class="text-center">deaths.</h4>
        <line-chart
          :chartData="arrDeaths"
          :options="chartOptions"
          label="Deaths"
          :chartColors="deathColors">
        </line-chart>
      </div>
    </div>

  </div>
</template>

<script>

import axios from "axios";
import moment from "moment";
import LineChart from "./components/LineChart";

export default {
  name: 'App',
  components: {
    LineChart
  },
  data() {
    return {

      arrPositive: [],
      positiveChartColors: {
        borderColor: "rgb(191,154,202)",
        pointBorderColor: "#0E1428",
        pointBackgroundColor: "rgba(191,154,202,0.7)",
        backgroundColor: "rgba(191,154,202,0.7)"
      },
      arrHospitalized: [],
      hospitalizedChartColors: {
        borderColor: "rgba(142,65,98,0.7)",
        pointBorderColor: "#260F26",
        pointBackgroundColor: "rgba(142,65,98,0.6)",
        backgroundColor: "rgba(142,65,98,0.6)"
      },
      arrInIcu: [],
      inIcuColors: {
        borderColor: "rgb(199,232,243)",
        pointBorderColor: "#190B28",
        pointBackgroundColor: "rgba(199,232,243,0.6)",
        backgroundColor: "rgba(199,232,243,0.6)"
      },
      arrOnVentilators: [],
      onVentilatorsColors: {
        borderColor: "rgba(65,57,62,0.6)",
        pointBorderColor: "#784F41",
        pointBackgroundColor: "rgba(65,57,62,0.5)",
        backgroundColor: "rgba(65,57,62,0.5)"
      },
      arrRecovered: [],
      recoveredColors: {
        borderColor: "rgb(237,162,192)",
        pointBorderColor: "#4E5E66",
        pointBackgroundColor: "rgba(237,162,192,0.6)",
        backgroundColor: "rgba(237,162,192,0.6)"
      },
      arrDeaths: [],
       deathColors: {
        borderColor: "rgba(64,42,44,0.9)",
        pointBorderColor: "#E06D06",
        pointBackgroundColor: "rgba(64,42,44,0.8)",
        backgroundColor: "rgba(64,42,44,0.8)"
      },
      chartOptions: {
        responsive:true,
        maintainAspectRatio: false
      }
    };
  },
  async created() {
    const { data } = await axios.get("https://covidtracking.com/api/us/daily");
    
    data.forEach(d => {

      const date = moment(d.date, "YYYYMMDD").format("MM/DD");
      const {
        positive,
        hospitalizedCurrently, 
        inIcuCurrently,
        onVentilatorCurrently,
        recovered,
        death
      } = d;

      this.arrPositive.push({ date, total: positive });
      this.arrHospitalized.push({ date, total: hospitalizedCurrently });
      this.arrInIcu.push({ date, total: inIcuCurrently });
      this.arrOnVentilators.push({ date, total: onVentilatorCurrently });
      this.arrRecovered.push({ date, total: recovered });
      this.arrDeaths.push({ date, total: death });

    })
  }
};

</script>

<style>
.text-center {
  font-family: Raleway
}
</style>
