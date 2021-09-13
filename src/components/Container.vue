<template>
  <v-container>
    <v-row>
      <v-col offset="4" cols="4">
        <H1>vue-diagram-demo</H1>
      </v-col>
    </v-row>
    <v-row>
      <v-col>
        <v-container>
          <v-row>
            <v-col v-for="(number,index) in inputArray" :key="index" cols="3">
              <v-text-field type="number" :value="number" @input="updateNumber($event,index)"></v-text-field>
            </v-col>
            <v-col cols="12">
              <v-btn @click="inputArray.push(0)">
                Add
              </v-btn>
            </v-col>
          </v-row>
        </v-container>
      </v-col>
    </v-row>
    <v-row>
      <v-col>
        <apexchart :options="chartOptionData" :series="chartOneSeries"></apexchart>
      </v-col>
      <v-col>
        <apexchart :options="chartOptionData" :series="chartTwoSeries"></apexchart>
      </v-col>
      <v-col>
        <apexchart :options="chartOptionData" :series="chartThreeSeries"></apexchart>
      </v-col>
    </v-row>
    <v-row>
      <v-col offset="4" cols="4">
        <h1>Auf 1 Chart:</h1>
      </v-col>
    </v-row>
    <v-row>
      <v-col>
        <apexchart :options="chartOptionData" :series="combinedSeries"></apexchart>
      </v-col>
    </v-row>
  </v-container>
</template>

<script>
import chart from "vue-apexcharts";
export default {
  name: "Container",
  components:{apexchart: chart},
  data: () => ({
    inputArray: [10,8,12],

    chartOneSeries: [{
      name: 'series-1',
      data: [10,8,12],
    }],
    chartTwoSeries:[],
    chartThreeSeries:[],

    convertedArray: [],
    outputArray: [],
    combinedSeries:[],
    startValue: 0,

    chartOptionData: {
      chart: {
        animations: {
          enabled: true,
          easing: 'easeinout',
          speed: 800,
          animateGradually: {
            enabled: true,
            delay: 150
          },
          dynamicAnimation: {
            enabled: true,
            speed: 350
          }
        }
      },
      yaxis:{
        min: -30,
        max: 30,
        forceNiceScale: true,
      },
    },
  }),
  methods:{
    updateNumber(event,index){
      this.inputArray[index] = parseInt(event);
      this.convertArray();
      this.unConvertArray();

      this.updateChartSeries();
    },
    convertArray(){//TODO 1
      let startArray = this.inputArray;

      //set Start Value
      this.startValue = startArray[0];

      //Parse Data
      let endData = [];
      let oldValue = startArray[0];
      for (let i = 0; i < startArray.length; i++){
        endData[i] = startArray[i] - oldValue;
        oldValue = startArray[i];
      }



      this.convertedArray = endData;
    },
    unConvertArray(){//TODO 2
      let startArray = this.convertedArray;

      //Parse Data
      let endData = [];
      let oldValue = this.startValue;
      for (let i = 0; i < startArray.length; i++){
        oldValue = oldValue + startArray[i];
        endData[i] = oldValue;
      }

      this.outputArray = endData;
    },
    updateChartSeries(){
      this.chartOneSeries = [{
        name: 'Start',
        data: this.inputArray,
      }];
      this.chartTwoSeries = [{
        name: 'Converted',
        data: this.convertedArray,
      }];
      this.chartThreeSeries = [{
        name: 'End',
        data: this.outputArray,
      }];
      this.combinedSeries = [
          this.chartOneSeries[0],
          this.chartTwoSeries[0],
          this.chartThreeSeries[0],
      ]

    }
  },
  created() {
    this.convertArray();
    this.unConvertArray();

    this.updateChartSeries();
  }
}
</script>

<style scoped>

</style>