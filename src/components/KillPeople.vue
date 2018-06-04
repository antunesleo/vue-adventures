<template>
  <div class="kill-people">
    <button type="button" class="btn btn-danger kill-button" v-on:click="killThemAll">Kill</button>

    <ul class="list-group">
      <li class="list-group-item list-group-item-danger"> Peoplpe I want to kill</li>
      <li class="list-group-item my-list-group-item" v-bind:key="person.id" v-for="person in people"> 
        <input type="checkbox" v-model="person.checked" class="form-check-input"> {{ person.name }}
      </li>
    </ul>
  </div>
</template>

<script>
  import axios from 'axios';

  export default {
    name: 'KillPeople',
    data () {
      return {
        people: [],
        errors: []
      }
    },
    methods: {
      killThemAll() {
        this.people = _.filter(this.people, function(person) {
          return !person.checked
        });
      },
      listPeople() {
        axios.get('http://localhost:5000/api/people').then(response => {
          console.log('passei', response);
          this.people = response.data;
        }).catch(e => {
          console.log('passei', e);
          this.errors.push('passei', e)
        });
      }
    },
    created()  {
      this.listPeople();
    }
  }
</script>

<style lang="scss">
.kill-people {
  margin: 10vh;
  height: 30vh;
  width: 50vh;
  text-align: left !important;

  .kill-button {
    margin-bottom: 2vh;
  }

  .my-list-group-item {
    padding-left: 6vh;
  }
}
