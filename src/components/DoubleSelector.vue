<template>
  <div class="double-selector row">
    <div class="col-6 col-sm-6 col-md-6">
      <ul class="selector list-group">
        <li class="list-group-item list-group-item-success">Ousados FC</li>
        <li class="list-group-item" v-for="item in leftList">  
            <button v-on:click="changeRight(item)" type="button" class="button-change-right btn btn-default">
                -->
            </button>
            {{ item.name }} 
        </li>
      </ul>
    </div>
    <div class="col-6 col-sm-6 col-md-6">
      <ul class="selector list-group">
        <li class="list-group-item list-group-item-success">Barcelona FC</li>
        <li class="list-group-item" v-for="item in rightList" v-if="!item.selected">  
            {{ item.name }} 
            <button v-on:click="changeLeft(item)" type="button" class="button-change-left btn btn-default">
                <--
            </button>
        </li>
      </ul>
    </div>

  </div>
</template>

<script>
  export default {
    name: 'DoubleSelector',
    data () {
      return {
        leftList: [
          {
            id: 1,
            name: "Leonardo"
          },
          {
            id: 2,
            name: "humberto"
          },
          {
            id: 3,
            name: "Breno"
          },
          {
            id: 4,
            name: "Brenner"
          },
          {
            id: 5,
            name: "Robson"
          }
        ],
        rightList: []
      }
    },
    methods: {
      changeRight(item) {
        this.leftList = _.reject(this.leftList, function(iteriItem) { return iteriItem.id == item.id; });
        this.rightList.push(item); 
      },
      changeLeft(item) {
        this.rightList = _.reject(this.rightList, function(iteriItem) { return iteriItem.id == item.id; });
        this.leftList.push(item);      
      }
    }
  }
</script>

<style lang="scss">
.double-selector  {
  padding: 5% 10% 5% 10%;
  .selector {
      margin: 0 auto;
      li {
          padding-left: 5%;
          padding-right: 5%;
      }
      .button-change-right {
          float: right;
      }
      .button-change-left {
          float: left;
      }
  }
}
</style>