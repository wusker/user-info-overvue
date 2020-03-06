<template>
  <div id="app">
    <h1>User Info</h1>
    <hr />
    <div class="userInfo">
      <span>Location: {{userInfo.location}}</span>
      <span>Time Zone: UTC-{{userInfo.timeZone}}</span>
      <span>Seconds on Page: {{userInfo.timeOnPage}}</span>
      <span>Current Page: {{userInfo.page}}</span>
      <span>Screen: {{userInfo.screenHeight}} x {{userInfo.screenWidth}}</span>
      <span>DPR: {{userInfo.dpr}}</span>
      <span>Browser Name: {{userInfo.browserName}}</span>
      <span>Browser Engine: {{userInfo.browserEngine}}</span>
      <span>Pages: {{userInfo.previous}}</span>
    </div>
  </div>
</template>

<script>
export default {
  name: "App",
  data() {
    return {
      userInfo: {
        dpr: devicePixelRatio,
        screenHeight: screen.height,
        screenWidth: screen.width,
        location: "",
        timeZone: new Date().getTimezoneOffset() / 60,
        timeOnPage: 0,
        page: window.location.pathname,
        browserName: navigator.appName,
        browserEngine: navigator.product,
        previous: history.length
      }
    };
  },
  mounted() {
    this.getUserLocation();

    setInterval(() => {
      this.startTimer();
    }, 1000);
  },
  methods: {
    async getUserLocation() {
      const url = "http://ip-api.com/json/";
      const response = await fetch(url);
      const result = await response.json();
      const { regionName, city } = result;
      this.userInfo.location = `${city}, ${regionName}`;
    },
    startTimer() {
      this.userInfo.timeOnPage += 1;
    }
  }
};
</script>

<style>
body {
  background: #05050c;
  display: flex;
  justify-content: center;
}
#app {
  font-family: -apple-system, BlinkMacSystemFont, system-ui, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #00d9ff;
  margin-top: 60px;
}
hr {
  margin: 3vw 0vw;
  border-color: #00d9ff;
}
.userInfo {
  display: flex;
  flex-direction: column;
}
.userInfo span {
  font-size: 1.5rem;
  font-weight: 500;
  margin-bottom: 0.5rem;
}
</style>
