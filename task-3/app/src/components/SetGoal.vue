<template>
  <div class="text-start">
    <label for="fromWhereCallSelector" class="form-label">
      Select from where you be call:
    </label>
    <select v-model="connectFrom" id="fromWhereCallSelector" class="form-select mb-2" aria-label="Select from where you be call">
      <option selected disabled>Open this select menu</option>
      <option v-for="(name, key) in countries" :key="key" :value="key">{{name}}</option>
    </select>
    <label for="toWhereCallSelector" class="form-label">
      Select to where you be call:
    </label>
    <select v-model="connectTo" id="toWhereCallSelector" class="form-select mb-2" aria-label="Select to where you be call">
      <option selected disabled>Open this select menu</option>
      <option v-for="(name, key) in countries" :key="key" :value="key">{{name}}</option>
    </select>
    <h2>Type of conection:</h2>
    <div>
      <div class="form-check">
        <input class="form-check-input" type="checkbox" value="0" id="flexCheckDefault" v-model="allWay">
        <label class="form-check-label" for="flexCheckDefault">
          All
        </label>
      </div>
      <div class="form-check">
        <input class="form-check-input" type="checkbox" value="1" id="flexCheckChecked" v-model="oneWay" :disabled="allWay">
        <label class="form-check-label" for="flexCheckChecked">
          Direct conection
        </label>
      </div>
      <div class="form-check">
        <input class="form-check-input" type="checkbox" value="2" id="flexCheckChecked" v-model="wayWithOneIntermediary" :disabled="allWay">
        <label class="form-check-label" for="flexCheckChecked">
          Through one intermediary
        </label>
      </div>
      <div class="form-check">
        <input class="form-check-input" type="checkbox" value="3" id="flexCheckChecked"  v-model="wayWithTwoIntermediary" :disabled="allWay">
        <label class="form-check-label" for="flexCheckChecked">
          Through two intermediary
        </label>
      </div>
    </div>
    <div class="mt-2 d-grid gap-2">
      <button class="btn btn-primary" type="button" @click="calculateData">Calculate</button>
    </div>
  </div>
</template>

<script lang="ts">
import { Component, Prop, Vue } from 'vue-property-decorator';

@Component
export default class SetGoal extends Vue {
  @Prop() private countries!: string;

  private connectFrom: string = '';
  private connectTo: string = '';
  private allWay: boolean = false;
  private oneWay: boolean = false;
  private wayWithOneIntermediary: boolean = false;
  private wayWithTwoIntermediary: boolean = false;

  private calculateData() {
    let connectFrom = this.connectFrom;
    if (connectFrom === 'us') {
      connectFrom = 'usa';
    }
    let connectTo = this.connectTo;
    if (connectTo === 'us') {
      connectTo = 'usa';
    }
    this.$emit('calculateData', {
      connectFrom,
      connectTo,
      allWay: this.allWay,
      oneWay: this.oneWay,
      wayWithOneIntermediary: this.wayWithOneIntermediary,
      wayWithTwoIntermediary: this.wayWithTwoIntermediary,
    });
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h3 {
  margin: 40px 0 0;
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
