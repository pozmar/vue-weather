<template>
  <div id="app" :class="this.big">
    <main>
      <div class="search-box">
        <input 
          type="text" 
          class="search-bar" 
          placeholder="Search..."
          v-model="query"
          @keypress="fetchWeather"
          @click="clear()"
        />
      </div>

      <div class="weather-container animate__animated animate__fadeInDown" v-if="typeof weather.main != 'undefined'">
        <div class="location-box">
          <div class="location">{{ weather.name }}, {{ weather.sys.country }}</div>
          <div class="date">{{ dateBuilder() }}</div>
        </div>

        <div class="weather-box">
          <div class="card-temp">
            <div class="inner">
              <div class="front">{{ Math.round(weather.main.temp) }}°c</div>
                <div class="back">
                  <div class="min">Min: {{ Math.round(weather.main.temp_min) }}°c</div>
                  <div class="max">Max: {{ Math.round(weather.main.temp_max) }}°c</div>
               </div>
              </div>
            </div>
              <div class="weather">{{ weather.weather[0].main }}</div>
            </div>
            
              <div class="description">
                <h3>Vincent Van Gogh</h3>
                <p>{{descriptionBuilder()}}</p>
            </div>
        
      </div>
      
      
    </main>
  </div>
</template>

<script>
export default {
  name: 'app',
  data () {
    return {
      api_key: '6e6e40554df6f4b359fe8aa195a5376c',
      url_base: 'https://api.openweathermap.org/data/2.5/',
      query: '',
      weather: {},
      big:''
    }
    
  },
  methods: {
    fetchWeather (e) {
      if (e.key == "Enter") {
        fetch(`${this.url_base}weather?q=${this.query}&units=metric&APPID=${this.api_key}`)
          .then(res => {
            return res.json();
          }).then(this.setResults).then(this.backg);
      }
    },
    clear(){
      return this.query = "";
    },
    setResults (results) {
     
      this.weather = results;
    },
    backg(){
      this.big = this.weather.weather[0].main.toLowerCase();
      console.log(this.big);
    },
    dateBuilder () {
      let current_datetime = new Date();
      let weekday =["Sunday", "Monday", "Tuesday", "Wednesday", "Thursday", "Friday", "Saturday"];
      let months = ["January", "February", "March", "April", "May", "June", "July", "August", "September", "October", "November","December"];

      let formatted_date = weekday[current_datetime.getDay()] + ", " + current_datetime.getDate() + " " + months[current_datetime.getMonth()] + " " + current_datetime.getFullYear();
      return formatted_date;
    },
    descriptionBuilder(){
      if(this.big =="clear"){
        console.log(this.big);
        
        return 'Olive Trees with Yellow Sky and Sun - 1889' ;
      }else if(this.big == 'snow'){
        return 'Landscape with Snow - 1888';
      }else if(this.big == "clouds"){
        return 'Wheatfields Under Thunderclouds - 1890';
      }else if(this.big =="rain"){
         return 'Landscape in the Rain - 1890';
        }else{
        return '';
      }
    }
    
  }
}

</script>

<style>

* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}
body {
  font-family: 'montserrat', sans-serif;
  background-color: #a0653b;
}

#app{
  background-image: url('./assets/snow-bg.jpg');
  background-size: cover;
  background-position: bottom;
  transition: 0.4s;
  width:95%;
  z-index:100;
  margin:20px auto;
  
}
#app.clear {
  background-image: url('./assets/sun-bg.jpg');
}
#app.clouds{
  background-image: url('./assets/clouds-bg.jpg');
}
#app.rain{
  background-image: url('./assets/rain-bg.jpg');
}
main {
  min-height: 95vh;
  padding: 20px;
  background-image: linear-gradient(to bottom, rgba(0, 0, 0, 0.25), rgba(0, 0, 0, 0.75));
}
.search-box {
  width: 100%;
  margin-bottom: 30px;
}
.search-box .search-bar {
  display: block;
  width: 10%;
  padding: 15px;
  outline: none;
  color: #313131;
  font-size: 20px;
  margin:0 auto;
  border:none;
  background: none;
  box-shadow: 0px 0px 8px rgba(0, 0, 0, 0.25);
  background-color: rgba(255, 255, 255, 0.5);
  transition: 1s;
}
.search-box .search-bar:focus {
  box-shadow: 0px 0px 16px rgba(0, 0, 0, 0.25);
  background-color: rgba(255, 255, 255, 0.75);
  border-radius: 20px;
  width:50%;
}
.location-box .location {
  color: #FFF;
  font-size: 32px;
  font-weight: 500;
  text-align: center;
  text-shadow: 1px 3px rgba(0, 0, 0, 0.25);
  
}
.location-box .date {
  color: #FFF;
  font-size: 20px;
  font-weight: 300;
  font-style: italic;
  text-align: center;
  margin:30px 0;
}
.weather-container {
  text-align: center;
  display: flex;
  justify-content: center;
  flex-direction: column;
  align-items: center;
}
.weather-container .front {
  color: #FFF;
  font-size: 102px;
  font-weight: 900;
  text-shadow: 3px 6px rgba(0, 0, 0, 0.25);
  background-color:transparent;
  border-radius: 16px;
  height:200px;
  
}
.min, .max{
  color: white;
}

.card-temp{
  background-color: transparent;
  width: 300px;
  height: 200px;
  perspective: 1000px; 
}
.inner{
  position: relative;
  width: 100%;
  height: 100%;
  text-align: center;
  transition: transform 0.8s;
  transform-style: preserve-3d;
}
.inner:hover{
  cursor: pointer;
}
.card-temp:hover .inner{
  transform: rotateY(180deg);

}
.card-temp:hover 
.front, .back{
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  backface-visibility: hidden;
  
}
.back{
  transform: rotateY(180deg);
  font-size: 50px;
  font-weight: 900;
  text-shadow: 3px 6px rgba(0, 0, 0, 0.25);
  background-image: linear-gradient(45deg, grey, white);
  border-radius: 16px;
  height:200px;
  
}
.weather-container .weather {
  color: #FFF;
  font-size: 48px;
  font-weight: 700;
  font-style: italic;
  text-shadow: 3px 6px rgba(0, 0, 0, 0.25);
  margin:30px 0;
}
.description{
  color:white;
  font-size:20px;
  font-style: italic;
  align-self: flex-end;
}

</style>