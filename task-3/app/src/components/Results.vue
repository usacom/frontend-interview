<template>
  <div class="text-start">
    <h2 class="text-center">List of ways</h2>
    <div class="card" v-for="(path, index) in pageOfItems" :key="index">
      <div class="card-body row justify-content-center">
        <div class="col-2">
          full cost: <br>
          <span class="badge rounded-pill bg-info text-dark">{{path['cost']}}$</span>
        </div>
        <div class="col">
          <span class="badge bg-primary">{{path['src']}}</span>
          <span class="position-relative">
            ({{path['name']}})->
            <span class="position-absolute top-0 start-100 translate-middle badge rounded-pill bg-info">
              {{path['price']}}$
            </span>
          </span>
          <span v-if="!!(Object.keys(path).indexOf('way')+1)">
            <span class="badge bg-primary">{{path['way']}}</span>
            <span class="position-relative">
              ({{path['way_name']}})->
              <span class="position-absolute top-0 start-100 translate-middle badge rounded-pill bg-info">
              {{path['price2']}}$
              </span>
            </span>
          </span>
          <span v-if="!!(Object.keys(path).indexOf('way2')+1)">
            <span class="badge bg-primary">{{path['way2']}}</span>
            <span class="position-relative">
              ({{path['des_name']}})->
              <span class="position-absolute top-0 start-100 translate-middle badge rounded-pill bg-info">
              {{path['price3']}}$
              </span>
            </span>
          </span>

          <span class="badge bg-primary">{{path['des']}}</span>
        </div>

        <div></div>
      </div>
    </div>
    <div class="mt-3 mb-3 row justify-content-center">
        <JwPagination class="justify-content-center d-flex" :items="showWay" :pageSize="4" :maxPages="3" @changePage="onChangePage"> </JwPagination>
    </div>

  </div>
</template>

<script lang="ts">
import JwPagination from 'jw-vue-pagination';
import { Component, Prop, Vue } from 'vue-property-decorator';

@Component({
  components: {
    JwPagination,
  },
})

export default class Result extends Vue {
  @Prop() private Data!: object;
  @Prop() private Companys!: object;
  private paths: object[] = [];
  private pageOfItems: object[] = [];


  get connectFrom() {
    return this.Data['connectFrom'];
  }
  get connectTo() {
    return this.Data['connectTo'];
  }
  get simpleWay(){
    let paths: any[] = [];
    Object.entries(this.Companys).forEach((company) => {
      company[1].forEach((path: any) => {
        if (this.connectFrom === path.src && this.connectTo ===  path.des) {
          path.name = company[0];
          paths.push({
                name: company[0],
                src: path.src,
                des: path.des,
                cost: path.price,
                price: path.price,
              });
        }
      });
    });
    paths.sort((pathA, pathB) => pathA.cost - pathB.cost);
    return paths;
  }

  get withOneIntermediary() {
    let paths: any[] = [];
    Object.entries(this.Companys).forEach((company) => {
      company[1].forEach((path: any) => {
        Object.entries(this.Companys).forEach((comp) => {
          company[1].forEach((way: any) => {
            if (this.connectFrom === path.src && this.connectTo !== path.des
              && this.connectTo === way.des && path.des === way.src) {

              paths.push({
                name: company[0],
                way_name: comp[0],
                src: path.src,
                way: path.des,
                des: way.des,
                price: path.price,
                price2: way.price,
                cost: path.price + way.price,
              });
            }
          });
        });
      });
    });
    paths.sort((pathA, pathB) => pathA.cost - pathB.cost);
    return paths;
  }

  get withTwoIntermediary() {
    let paths: any[] = [];
    Object.entries(this.Companys).forEach((company) => {
      company[1].forEach((path: any) => {
        Object.entries(this.Companys).forEach((comp) => {
          company[1].forEach((way: any) => {

            Object.entries(this.Companys).forEach((item) => {
              company[1].forEach((des: any) => {
                if (this.connectFrom === path.src && this.connectTo !== path.des
                  && this.connectTo !== way.des && path.des === way.src
                  && way.des === des.src && this.connectTo === des.des) {

                  paths.push({
                    name: company[0],
                    way_name: comp[0],
                    des_name: item[0],
                    src: path.src,
                    way: path.des,
                    way2: way.des,
                    des: des.des,
                    price: path.price,
                    price2: way.price,
                    price3: des.price,
                    cost: path.price + way.price + des.price,
                  });
                }
              });
            });
          });
        });
      });
    });
    paths.sort((pathA, pathB) => pathA.cost - pathB.cost);
    return paths;
  }

  get allWay() {
    let paths: any[] = this.simpleWay.concat(...this.withOneIntermediary, ...this.withTwoIntermediary);
    paths.sort((pathA, pathB) => pathA.cost - pathB.cost);
    return paths;
  }

  get showWay() {
    if (this.Data['allWay'] === true) {
      return this.allWay;
    }
    if (this.Data['oneWay'] === true
      && this.Data['wayWithOneIntermediary'] === true
      && this.Data['wayWithTwoIntermediary'] === true) {
      return this.allWay;
    }
    if (this.Data['oneWay'] === true
      && this.Data['wayWithOneIntermediary'] !== true
      && this.Data['wayWithTwoIntermediary'] !== true) {
      return this.simpleWay;
    }
    if (this.Data['oneWay'] === true
      && this.Data['wayWithOneIntermediary'] === true
      && this.Data['wayWithTwoIntermediary'] !== true) {
      let paths: any[] = this.simpleWay.concat(...this.withOneIntermediary);
      paths.sort((pathA, pathB) => pathA.cost - pathB.cost);
      return paths;
    }
    if (this.Data['oneWay'] === true
      && this.Data['wayWithOneIntermediary'] !== true
      && this.Data['wayWithTwoIntermediary'] === true) {
      let paths: any[] = this.simpleWay.concat(...this.withTwoIntermediary);
      paths.sort((pathA, pathB) => pathA.cost - pathB.cost);
      return paths;
    }
    if (this.Data['oneWay'] !== true
      && this.Data['wayWithOneIntermediary'] === true
      && this.Data['wayWithTwoIntermediary'] === true) {
      let paths: any[] = this.withOneIntermediary.concat(...this.withTwoIntermediary);
      paths.sort((pathA, pathB) => pathA.cost - pathB.cost);
      return paths;
    }
    if (this.Data['oneWay'] !== true
      && this.Data['wayWithOneIntermediary'] === true
      && this.Data['wayWithTwoIntermediary'] !== true) {
      return this.withOneIntermediary;
    }
    if (this.Data['oneWay'] !== true
      && this.Data['wayWithOneIntermediary'] !== true
      && this.Data['wayWithTwoIntermediary'] === true) {
      return this.withTwoIntermediary;
    }
    return [];
  }
  private onChangePage(pageOfItems: any[]){
    this.pageOfItems = pageOfItems;
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>

</style>
