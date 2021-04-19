<template>
  <div>
    <div class="header"></div>
    <div class="container">
      <div class="heading mb-5">
        <h3 class="mb-4 font-weight-bold text-light">Users Ranks</h3>

        <div class="row">
          <div class="col-3">
            <select
              class="custom-select"
              name="sortBy"
              id="sortBy"
              v-model="sortBy"
            >
              <option value="highest">highest rank</option>
              <option value="lowest">lowest rank</option>
            </select>
          </div>
          <div class="col-2">
            <input
              v-model="maxRank"
              class="form-control mr-sm-2"
              type="number"
              placeholder="Max Rank"
              aria-label="maxRank"
            />
          </div>
          <div class="col-7">
            <input
              v-model="searchValue"
              class="form-control mr-sm-2"
              type="search"
              placeholder="Search by name"
              aria-label="Search"
            />
          </div>
        </div>
      </div>
      <table class="table table-striped table-hover">
        <thead>
          <tr>
            <th scope="col"></th>
            <th scope="col">Rank</th>
            <th scope="col">Name</th>
            <th scope="col">Email</th>
            <th scope="col">City</th>
          </tr>
        </thead>
        <tbody>
          <tr v-for="user in filteredUsers" v-bind:key="user.id">
            <td>
              <div class="rank-arrow" v-on:click="() => rankUp(user)">
                &#8657;
              </div>
              <div class="rank-arrow" v-on:click="() => rankDown(user)">
                &#8659;
              </div>
            </td>
            <th scope="row">{{ user.rank }}</th>
            <td>{{ user.name }}</td>
            <td>{{ user.email }}</td>
            <td>{{ user.address.city }}</td>
          </tr>
        </tbody>
      </table>
    </div>
  </div>
</template>

<script>
// import axios from "axios";
import usersRecord from "../lib/users.json";

export default {
  name: "Users",
  data() {
    return {
      users: [],
      sortBy: "highest",
      searchValue: "",
      maxRank: null
    };
  },
  created: function() {
    // axios.get("../lib/users.json").then(res => {
    //   this.users = res.data;
    // });
    this.users = usersRecord;
  },
  computed: {
    filteredUsers() {
      let tempUsers = this.users;

      // Process search input
      if (this.searchValue != "" && this.searchValue) {
        tempUsers = tempUsers.filter(item => {
          return item.name
            .toLowerCase()
            .includes(this.searchValue.toLowerCase());
        });
      }

      // Sort by rank
      tempUsers = tempUsers.sort((a, b) => {
        if (this.sortBy === "highest") {
          return a.rank - b.rank;
        } else if (this.sortBy === "lowest") {
          return b.rank - a.rank;
        }
      });

      // Filter out by ranl time
      if (this.maxRank)
        tempUsers = tempUsers.filter(item => {
          return item.rank <= this.maxRank;
        });

      // Show sorted array in descending or ascending order
      if (!this.ascending) {
        tempUsers.reverse();
      }

      return tempUsers;
    }
  },
  methods: {
    rankUp: function(selectedUser) {
      let index = this.users.indexOf(selectedUser);
      this.users[index].rank += 1;
    },
    rankDown: function(selectedUser) {
      let index = this.users.indexOf(selectedUser);
      this.users[index].rank -= 1;
    }
  }
};
</script>

<style>
h3 {
  margin-bottom: 5%;
}

.rank-arrow {
  cursor: pointer;
}

.header {
  width: 100%;
  height: 200px;
  background-color: #0e1e25;
  margin-bottom: -120px;
}
</style>
