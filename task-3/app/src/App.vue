<template>
  <div id="app">
    <h1>Searching the most profitable way</h1>
    <div class="container">
      <div class="row">
        <div class="col-12 col-md-5 mb-3">
          <SetGoal v-bind:countries="countries" v-on:calculateData="calledCalculateData"/>
        </div>
        <div class="col-12 col-md-7">
          <Results ref="resultComponent" v-if="showResults" v-bind:Data="dataForCalculate" v-bind:Companys="companys"/>
        </div>

      </div>

    </div>

  </div>
</template>

<script lang="ts">
import { Component, Vue } from 'vue-property-decorator';
import SetGoal from './components/SetGoal.vue';
import Results from './components/Results.vue';
import TestData from '../call-paths.json';

@Component({
  components: {
    SetGoal,
    Results,
  },
})
export default class App extends Vue {
  private countries = TestData.data.country;
  private companys = TestData.data.company;
  private showResults: boolean = false;
  private dataForCalculate: object = {};

  private calledCalculateData(value: object) {
    this.showResults = false;
    this.dataForCalculate = value;
    this.showResults = true;
  }
}
</script>

<style lang="scss">
@import "~bootstrap/scss/bootstrap";
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
