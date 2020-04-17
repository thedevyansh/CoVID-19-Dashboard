<template>
  <div id="app">
  </div>
</template>

<script>

import axios from "axios";
import moment from "moment";

export default {
  name: 'App',
  components: {

  },
  data() {
    return {

      arrPositive: [],
      arrHospitalized: [],
      arrInIcu: [],
      arrOnVentilators: [],
      arrRecovered: [],
      arrDeaths: []
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
</style>
