<template>
  <div class="data-table">
    <form class="form-inline input-search">
      <input v-model="search" class="form-control mr-sm-2" type="search" placeholder="Search" aria-label="Search">
    </form>
    <table class="table table-striped">
      <thead>
        <tr>
          <th scope="col"><button class="btn-transparent">ID</button></th>
          <th scope="col"><button class="btn-transparent" v-on:click="sortColumn('name')">Name {{ sortIcon }}<span class="glyphicon glyphicon-align-left" aria-hidden="true"></span>  </button></th>
          <th scope="col"><button class="btn-transparent" v-on:click="sortColumn('lastName')">Last Name {{ sortIcon }}</button></th>
          <th scope="col"><button class="btn-transparent" v-on:click="sortColumn('username')">Username {{ sortIcon }}</button></th>
          <th scope="col"><button class="btn-transparent" v-on:click="sortColumn('age')">Age {{ sortIcon }}</button></th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="user in currentPageUsers">
          <td scope="row">{{ user.id }}</td>
          <td>{{ user.name }}</td>
          <td>{{ user.lastName }}</td>
          <td>{{ user.username }}</td>
          <td>{{ user.age }}</td>
        </tr>
      </tbody>
    </table>
    <div>
      <ul class="pagination">
        <li class="page-item">
          <button class="page-link" tabindex="-1" v-on:click="goToPreviousPage">Previous</button>
        </li>
        <li class="page-item" v-bind:class="{active: (currentPage == page.number)}" v-for="page in paginatedAndFilteredUsers">
          <button class="page-link"  v-on:click="goToPage(page.number)">{{ page.number }}</button>
        </li>
        <li class="page-item">
          <button class="page-link" v-on:click="goToNextPage">Next</button>
        </li>
      </ul>
    </div>
  </div>
</template>

<script>
import axios from 'axios';

export default {
  name: 'DataTable',
  data () {
    return {
      search: '',
      users: [], 
      currentPage: 1,
      usersPerPage: 5,
      totalPages: 0,
      currentSortType: {
        name: "asc",
        lastName: "asc",
        username: "asc",
        age: "asc"
      }
    }
  },
  created() {
    axios.get('http://localhost:5000/api/users').then(response => {
      console.log('passei', response);
      this.users = response.data;
    }).catch(e => {
      console.log('passei', e);
      this.users = [
        {
          "id": 1,
          "name": "Leonardo",
          "lastName": "Coelho",
          "username": "antunesleo",
          "age": 22
        },
        {
          "id": 2,
          "name": "Breno",
          "lastName": "Brenalves",
          "username": "breno",
          "age": 26
        },
        {
          "id": 3,
          "name": "Edmar",
          "lastName": "Coelho",
          "username": "edmar.coelho",
          "age": 53
        }
      ];
    });
    console.log('linha depois da request')
  },
  methods: {
    goToPage(pageNumber) {
      this.currentPage = pageNumber;
    },
    goToPreviousPage() {
      if (this.currentPage  > 1) {
        this.currentPage--;
      }
    },
    goToNextPage() {
      if (this.currentPage < this.totalPages) {
        this.currentPage++;
      }
    },
    sortColumn(columnName) {
      let inverseSortType = this.currentSortType[columnName] === "asc" ? "desc" : "asc";
      this.currentSortType[columnName] = inverseSortType;
      this.users = _.orderBy(this.users, [columnName], [inverseSortType]);
    }
  },
  computed: {
    filteredUsers() {
      return this.users.filter(user => {
        let name = user.name.toLowerCase().indexOf(this.search.toLowerCase()) > -1;
        let lastName = user.lastName.toLowerCase().indexOf(this.search.toLowerCase()) > -1;
        let username = user.username.toLowerCase().indexOf(this.search.toLowerCase()) > -1;
        let age = user.age.toString().indexOf(this.search) > -1;
        return name || username || age || lastName;
      })
    },
    totalUsers() {
      return this.filteredUsers.length;
    },
    paginatedAndFilteredUsers() {
      if (this.totalUsers > this.usersPerPage) {
        let pagination = [];
        this.totalPages = this.totalUsers / this.usersPerPage;
        let chunkUsers = _.chunk(this.filteredUsers, this.usersPerPage);
        for ([index, users] of chunkUsers.entries()) {
          let page = {
            number: index + 1,
            users: users
          }
          pagination.push(page);
        }
        return pagination;
      }
      return [{number: 1, users: this.filteredUsers}]
    },
    currentPageUsers() {
      let currentPageIndex = this.currentPage - 1;
      let page = this.paginatedAndFilteredUsers[currentPageIndex];
      return page.users;  
    },
    sortIcon() {
      return this.currentSortType['name'] === 'asc' ? '^' : 'v';
    }
  }
}
</script>

<style lang="scss">
.data-table {
  padding: 5% 10% 5% 10%;
  .input-search {
    margin-bottom: 5vh;
  }

  .btn-transparent {
    background-color: transparent;
    position: relative;
    width: 100%;
    height: 100%;
    border: 0;
    padding: 2vh;
    font-weight: bold;
    color: #2c3e50;
  }
  
  .btn-transparent:focus {
    outline: none;
  }
}

th {
  padding: 0 !important;
}
</style>
