<template>
  <div id="app">
    <!-- <img src="./assets/logo.png"> -->
    <div class="topnav">
      <h4 class="labelHearder">Double Calculator</h4>
    </div>
    <CalculateDetail />
  </div>
</template>

<script>
// import HelloWorld from './components/HelloWorld'
import CalculateDetail from "./components/calculateDetail";
export default {
  name: "App",
  components: {
    CalculateDetail
  }
};
</script>

<style>
#app {
  font-family: "Avenir", Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  /* margin-top: 60px; */
}
.topnav {
  overflow: hidden;
  background-color: #fff;
  padding-bottom: 15px;
  margin-bottom: 40px;
  box-shadow: 0 4px 8px 0 rgba(0, 0, 0, 0.1);
}
.labelHearder {
  text-align: left;
  padding-top: 25px;
  padding-left: 15px;
  
}
</style>
