<template>
  <section class="jumbotron">
    <h3 class="jumbotron-heading">Search Github Users</h3>
    <div>
      <input
        type="text"
        placeholder="enter the name you search"
        v-model="KeyWord"
      />&nbsp;
      <button @click="searchUsers">Search</button>
    </div>
  </section>
</template>

<script>
import axios from "axios";
export default {
  name: "Search",
  data() {
    return {
      KeyWord: "",
    };
  },
  methods: {
    searchUsers() {
      this.$bus.$emit("updateListData", {
        isFirst: false,
        isLoading: true,
        errMsg: "",
        users: [],
      });
      axios.get(`http://api.github.com/search/users?q=${this.KeyWord}`).then(
        (response) => {
          console.log("成功");
          this.$bus.$emit("updateListData", {
            isLoading: false,
            errMsg: "",
            users: response.data.items,
          });
        },
        (error) => {
          console.log("失败", error.message);
          this.$bus.$emit("updateListData", {
            isLoading: false,
            errMsg: error.message,
            users: [],
          });
        }
      );
    },
  },
};
</script>

<style>
</style>