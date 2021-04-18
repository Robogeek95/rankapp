<template>
  <div class="container">
    <div class="heading mb-4">
      <h3>Users Ranks</h3>
      <select name="sortBy" id="select" v-model="sortBy">
        <option value="highest">highest rank</option>
        <option value="lowest">lowest rank</option>
      </select>
    </div>
    <table class="table table-striped">
      <thead>
        <tr>
          <th scope="col">Rank</th>
          <th scope="col">Name</th>
          <th scope="col">Email</th>
          <th scope="col">City</th>
          <th scope="col"></th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="user in filteredUsers" v-bind:key="user.id">
          <th scope="row">{{ user.rank }}</th>
          <td>{{ user.name }}</td>
          <td>{{ user.email }}</td>
          <td>{{ user.address.city }}</td>
          <td>
            <div class="rank-arrow" v-on:click="() => rankUp(user)">
              &#8657;
            </div>
            <div class="rank-arrow" v-on:click="() => rankDown(user)">
              &#8659;
            </div>
          </td>
        </tr>
      </tbody>
    </table>
  </div>
</template>

<script>
// import axios from "axios";
import usersRecord from "../lib/users.json";

export default {
  name: "Users",
  data() {
    return {
      user: [],
      sortBy: "highest"
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
          return item.title
            .toUpperCase()
            .includes(this.searchValue.toUpperCase());
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
      console.log(this.users[index]);
      this.users[index].rank += 1;
      console.log(this.users[index]);
    },
    rankDown: function(user) {
      console.log(user);
      user.rank -= 1;
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
</style>
