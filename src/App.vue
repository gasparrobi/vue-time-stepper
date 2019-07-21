<template>
  <div id="app">
    <img width="25%" height="25%" src="./assets/logo.png" />
    <TimeStepper
      :hour.sync="hour"
      :minute.sync="minute"
      :min-hour="minHour"
      :max-hour="maxHour"
      :minute-interval="minuteInterval"
    ></TimeStepper>
  </div>
</template>

<script>
import TimeSelect from "./components/TimeSelect";
import TimeStepper from "./components/TimeStepper";

export default {
  name: "App",
  components: {
    TimeSelect,
    TimeStepper
  },
  data: () => ({
    hour: 12,
    minute: 0,
    minHour: 0,
    maxHour: 23,
    minuteInterval: 15
  })
};
</script>

<style>
* {
  box-sizing: border-box;
}

#app {
  font-family: "Avenir", Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
}

#app img {
  max-width: 150px;
  margin-bottom: 30px;
}
</style>
