<template>
  <div id="app">
    <div class="app__bg">
      <p>{{ region.country_name }}, {{ region.country_code }}</p>
      <p>{{ region.time_zone }}</p>
      <p>IP: {{ region.ip }}</p>
      <br />
      <p>Day of Week: {{ time.day_of_week }}</p>
      <p>Day of Year: {{ time.day_of_year }}</p>
      <p>Week Number: {{ time.week_number }}</p>
      <p>Time: {{ currentTime }}</p>
    </div>
  </div>
</template>

<script>
import axios from "axios";

export default {
  name: "App",
  components: {},
  data() {
    return {
      ipApi: "https://freegeoip.app/json/",
      timeApi: "http://worldtimeapi.org/api/timezone",
      region: "",
      country: "",
      city: "",
      time: "",
      currentTime: "",
    };
  },
  async created() {
    await this.getRegion();
    await this.getTime();
    //this.updateTime();
  },
  methods: {
    async getRegion() {
      await axios
        .get(this.ipApi)
        .then((res) => {
          this.region = res.data;
          let timeZone = res.data.time_zone;
          let split = timeZone.split("/");
          this.country = split[0];
          this.city = split[1];
        })
        .catch((err) => console.log(err));
    },
    async getTime() {
      await axios
        .get(this.timeApi + "/" + this.country + "/" + this.city)
        .then((res) => {
          this.time = res.data;
          console.log(this.country);
          let date = new Date();
          let current = date.getHours() + ":" + date.getMinutes();
          this.currentTime = current;
        });
      //this.updateTime();
    },
    /*updateTime() {
      //let interval = (60 - new Date().getSeconds()) * 1000 + 5;
      setInterval(this.getTime(), 1000);
    },*/
  },
};
</script>

<style lang="scss">
@import url("https://fonts.googleapis.com/css2?family=Inter:wght@300;600;900&display=swap");
#app {
  font-family: "Inter", sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #ffffff;
  font-size: 16px;
}
.app__bg {
  background: url("./assets/desktop/bg-image-daytime.jpg");
  position: fixed;
  top: 0;
  left: 0;
  width: 100vw;
  height: 100vh;
  font-weight: 600;
  font-size: 1.2em;
}
@media only screen and (max-width: 375px) {
  .app__bg {
    background: url("./assets/mobile/bg-image-daytime.jpg") no-repeat;
  }
}
</style>
