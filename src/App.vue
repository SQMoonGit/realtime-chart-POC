<template>
  <div id="small">
    <line-chart :chart-data="datacollection" id="mychart"></line-chart>
  </div>
</template>

<script lang="ts">
import LineChart from "./LineChart.js";
import { io } from "socket.io-client";
import { Component, Vue } from "vue-property-decorator";

const socket = io("http://localhost:4000");
@Component({
  components: { LineChart },
})
export default class App extends Vue {
  private datacollection: any = null;

  created() {
    this.getRealtimeData();
  }

  public fillData(fetchedData: any) {
    this.datacollection = {
      labels: [
        this.getRandomChartValues(fetchedData),
        this.getRandomChartValues(fetchedData),
      ],
      datasets: [
        {
          label: "Google Stock",
          backgroundColor: "blue",
          data: [
            this.getRandomChartValues(fetchedData),
            this.getRandomChartValues(fetchedData),
          ],
        },
        {
          label: "Apple Stock",
          backgroundColor: "yellow",
          data: [
            this.getRandomChartValues(fetchedData),
            this.getRandomChartValues(fetchedData),
          ],
        },
      ],
    };
  }

  public getRealtimeData() {
    socket.on("newdata", (fetchedData) => {
      this.fillData(fetchedData);
    });
  }

  public getRandomChartValues(digit: number) {
    return Math.floor(Math.random() * digit);
  }
}
</script>

<style>
#small {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  /* margin-top: 60px; */
  max-width: 600px;
  margin: 150px auto;
}
</style>
