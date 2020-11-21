<template>
  <div id="app" :class="typeof weather.main != 'undefined' && weather.main.temp > 16? 'warm' : ''">
    <main>
      <div class="search-box">
        <input
          type="text"
          name=""
          class="search-bar"
          placeholder="Search..."
          id=""
          v-model="query"
          @keypress="fetchWeather"
        />
      </div>
      <div class="weather-wrap" v-if="typeof weather.main != 'undefined'">
        <div class="location-box">
          <div class="location">{{ weather.name }}, {{ weather.sys.country}}</div>
          <div class="date">{{dateBuilder()}}</div>
          <div class="weather-box">
            <div class="temp">{{ Math.round(weather.main.temp)}}°C</div>
            <div class="weather">{{weather.weather[0].main}}</div>
          </div>

        </div>
      </div>
    </main>
  </div>
</template>

<script>
export default {
  name: "App",
  components: {},
  data() {
    return {
      api_key: '0bc244480c09fcea11d87c1a31da4c15',
      url_base: 'http://api.openweathermap.org/data/2.5/',
      query: '',
      weather: {}

    };
  },
  methods: {
    fetchWeather(e) {
      if(e.key == "Enter"){
        fetch(`${this.url_base}weather?q=${this.query}&units=metric&APPID=${this.api_key}`)
        .then( res => {return res.json()})
        .then(this.setResults)
      }
    },
    setResults(results){
      console.log(results)
      this.weather = results;
      
    },
    dateBuilder(){
      let d = new Date();
      let months = ["Enero", "Febrero", "Marzo", "Abril", "Mayo", "Junio", "Julio", "Agosto","Septiembre", "Octubre", "Noviembre", "Diciembre"]
      let days = ["Domingo","Lunes","Martes","Miercoles","Jueves","Viernes","Sabado"]

      let day = days[d.getDay()];
      let date = d.getDate();
      let month = months[d.getMonth()]
      let year = d.getFullYear();

      return `${day} ${date} ${month} ${year}`
    }
  }
};
</script>

<style>
@import url('https://fonts.googleapis.com/css2?family=Montserrat:ital,wght@0,300;0,500;0,900;1,300;1,700&display=swap');
* {
  padding: 0;
  margin: 0;
  box-sizing: border-box;
}
body{
  font-family: 'Montserrat', sans-serif;
}

#app.warm{
    background-image: url('assets/warm-bg.jpg');

}
#app {
  background-image: url("assets/cold-bg.jpg");
  
  background-size: cover;
  transition: 0.4s;
}
main {
  min-height: 100vh;
  padding: 25px;
  background-image: linear-gradient(to bottom, rgba(0,0,0,0.25), rgba(0,0,0,0.75));
}
.search-box {
  width: 100%;
  margin-bottom: 30px;
}
.search-box .search-bar{
  display: block;
  width: 100%;
  padding: 15px;
  color: #313131;
  font-size: 20px;
  
  appearance: none;
  border: none;
  outline: none;
  background: none;
  background-color: rgba(255, 255, 255, 0.5);
  box-shadow: 0px 0px 8px 0px;
  border-radius: 0px 16px 0px 16px;
  transition: 0.2s;

}

.search-box .search-bar:focus{

  background-color: rgba(255, 255, 255, 0.75);
  box-shadow: 0px 0px 16px 0px;
  border-radius: 16px 0px 16px 0px;

}

.location-box .location{
  color: #fff;
  font-size: 32px;
  font-weight: 500;
  text-align: center;
  text-shadow: 1px 3px rgba(0, 0, 0, 0.25);
}
.location-box .date{
  color: #fff;
  font-size: 20px;
  font-weight: 300;
  font-style: italic;
  text-align: center;
}
.weather-box{
  text-align: center;
}
.weather-box .temp{
  display: inline-block;
  padding: 10px 25px;
  background-color: rgba(255, 255, 255, 0.2);
  text-shadow: 1px 3px rgba(0, 0, 0, 0.25);
  box-shadow: 4px 4px rgba(0, 0, 0, 0.5);
  border-radius: 32px 32px;
  color: #fff;
  font-size: 102px;
  font-weight: 900;
  margin: 16px;
}
.weather-box .weather{
  color: #FFF;
  font-size: 48px;
  font-weight: 700;
    text-shadow: 1px 3px rgba(0, 0, 0, 0.25);
  font-style: italic;
}
</style>
