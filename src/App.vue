<template>
  <div id="app" :class="[typeof weather.main != 'undefined' && weather.main.temp > 20 ? null : cold]">
    <div class="container">
      <div class="items-container">
        <div class="search-box">
          <input
           placeholder="Search Country or City"
            name="country search" 
            v-model="query"
            @keypress="fetchWeather"
            @keyup.enter="hideKeyboard"
            
             />
        </div>
        <div class="info-container" v-if="typeof weather.main != 'undefined'">
          <div class="weather-wrap" >
            <div class="location-box">
              <div class="location">{{weather.name}}</div>
              <div class="date">{{dateBuilder()}}</div>
            </div>
          </div>
          <div class="weather-box">
            <div class="weather">{{weather.weather[0].description}}</div>
            <div class="temp">{{weather.main.temp}} °</div>
          </div>
        </div>
        <p v-else class="warning">That is neither a Country nor City <br>
        Check your spelling</p>
      </div>
    </div>

  </div>
</template>

<script>

export default {
  name: "App",
  components: {},
  data() {
    return {
      api_key: "61028cdf9f7d04ae8acd164e371977a5",
      base_url: 'https://api.openweathermap.org/data/2.5/',
      query: '',
      weather: {},
     cold: 'cold'
    };
  },
  methods:{
fetchWeather(e){
 if(e.key=="Enter"){
    fetch(`${this.base_url}weather?q=${this.query}&units=metric&appid=${this.api_key}`)
    .then(res =>{
      return res.json();
    })
    .then(this.setResults)
  }
},
hideKeyboard(){
  document.activeElement.blur();
},
setResults(results){
  this.weather = results;
},
 dateBuilder () {
      let d = new Date();
      let months = ["January", "February", "March", "April", "May", "June", "July", "August", "September", "October", "November", "December"];
      let days = ["Sunday", "Monday", "Tuesday", "Wednesday", "Thursday", "Friday", "Saturday"];
      let day = days[d.getDay()];
      let date = d.getDate();
      let month = months[d.getMonth()];
      let year = d.getFullYear();
      return ` ${month} ${date}, (${day}) ${year}`;
    }
  },
  mounted(){

  }
};
</script>

<style lang="scss">
* {
  padding: 0;
  margin: 0;
  font-family: "Roboto", sans-serif;
}
#app {
  min-height: 100vh;
  background-image: url("./assets/img/warm-bg.jpg");
  background-size: cover;
  background-position: center;
  background-repeat: no-repeat;
  transition: 0.5s ease-in-out;
}
#app.cold{
  background-image: url("./assets/img/cold-bg.jpg");
}
.container {
  display: flex;
  justify-content: center;
  background-image: linear-gradient(rgba(0, 0, 0, 0.4), rgba(0, 0, 0, 0.7));
   min-height: 100vh;
  width: 100%;

  .items-container {
    margin-top: 100px;
    width: 800px;
  }
}
.search-box {
  display: flex;
  justify-content: center;
  input {
    padding: 10px 0;
    width: 500px;
    font-size: 20px;

    border: none;
    text-align: center;
    outline: none;
    color: white;
    letter-spacing: 2.5px;
    font-weight: bold;
    border-radius: 10px;
    background: linear-gradient(
      145deg,
      rgba(213, 213, 213, 0.157),
      rgba(254, 254, 254, 0.712)
    );
    box-shadow: 33px 33px 96px #7b7b7b, -33px -33px 96px #ffffff;
    &::placeholder {
      color: rgba(255, 255, 255, 0.8);
      font-weight: normal;
    }
  }
}
.info-container {
  display: flex;
  flex-wrap: wrap;
  justify-content: space-around;
  padding: 0 16px;
}
.location-box,
.weather-box {
  background: linear-gradient(
    145deg,
    rgba(213, 213, 213, 0.157),
    rgba(254, 254, 254, 0.308)
  );
  padding: 30px;
  width: 232px;
  margin-top: 50px;
}

.location,
.weather {
  color: white;
  font-size: 32px;
  font-weight: bold;
  text-align: center;
  text-transform: capitalize;
}

.date,
.temp {
  color: rgba(255, 255, 255, 0.89);
  font-size: 20px;
  text-align: center;
  margin-top: 15px;
  text-align: center;
}

.warning{
  text-align: center;
  font-size: 40px;
  font-weight: bold;
  color: white;
  margin-top: 50px;
}

@media screen and (max-width: 700px) {
  .search-box {
  input {
  width: 90vw;
  }
  }
}
</style>
