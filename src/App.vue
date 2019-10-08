<template>
  <div id="app">
    <div class="container">
      <Search></Search>
      <usersMain></usersMain>
    </div>
  </div>
</template>

<script>
import Search from "./components/Search";
import usersMain from "./components/Main";

export default {
  name: "App",
  components: {   //映射成标签
    Search,
    usersMain
  }
};
</script>

<style>
</style>
