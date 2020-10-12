<template>

  <div>
    <center>
    <div class="container">
      <button @click="getLocation" class="btn btn-primary">Get Your Location Weather Infomration</button>
      <br><br>
      <div class="container" style="background-color:#f1f1f1" v-if="locfound">
        <center>
          <img v-bind:src="icon" alt="iconweather" style="width:100px">
        </center>
        <h4> <b> {{ locname }} </b> </h4>
        <h4>{{ locweatherdesc }}</h4>
        <h5> {{ lattitude }} {{ longitude }}</h5>
        <h4>{{ loctemp }}</h4>
        <h4>{{ lochumidity }}</h4>
        <h5>{{ locwind }}</h5>
      </div>
    </div>
    </center>

    <div class="cards" v-if="locfound">
      <div class="card" >
        <h4> <b>Forecast +1 Day</b> </h4>
        <center>
        <img v-bind:src="ficon0" alt="ii"> <br>
        {{ flocweatherdesc[0] }} <br>
        {{ ftemp[0] }} <br>
        {{ fhumidity[0] }} <br>
        </center>

      </div>
      <div class="card">
        <h4> <b>Forecast +2 Day</b> </h4>
        <center>
        <img v-bind:src="ficon1" alt="ii"> <br>
        {{ flocweatherdesc[1] }} <br>
        {{ ftemp[1] }} <br>
        {{ fhumidity[1] }} <br>
        </center>
      </div>
      <div class="card">
        <h4> <b>Forecast +3 Day</b> </h4>
        <center>
        <img v-bind:src="ficon2" alt="ii"> <br>
        {{ flocweatherdesc[2] }} <br>
        {{ ftemp[2] }} <br>
        {{ fhumidity[2] }} <br>
        </center>
      </div>
      <div class="card">
        <h4> <b>Forecast +4 Day</b> </h4>
        <center>
        <img v-bind:src="ficon3" alt="ii"> <br>
        {{ flocweatherdesc[3] }} <br>
        {{ ftemp[3] }} <br>
        {{ fhumidity[3] }} <br>
        </center>
      </div>
      <div class="card">
        <h4> <b>Forecast +5 Day</b> </h4>
        <center>
        <img v-bind:src="ficon4" alt="ii"> <br>
        {{ flocweatherdesc[4] }} <br>
        {{ ftemp[4] }} <br>
        {{ fhumidity[4] }} <br>
        </center>
      </div>
        <div class="card">
        <h4> <b>Forecast +6 Day</b> </h4>
        <center>
        <img v-bind:src="ficon5" alt="ii"> <br>
        {{ flocweatherdesc[5] }} <br>
        {{ ftemp[5] }} <br>
        {{ fhumidity[5] }} <br>
        </center>
      </div>
    </div>
        
    
    <center>
        <div class="container" style="background-color:#f1f1f1"> 
            <button type="button" v-on:click="resetWeather" class="cancelbtn">Reset</button> 
        </div>
    </center> 

    
  </div>
</template>

<script>
export default {
  data () {
    return {
      // cityname: '',
      // key: '',
      // temp: '',
      // weatherText: '',
      // ppt: false,
      // rain: '',
      // raintype: '',
      // icon: '',
      // gotweather: false,
      lat: '',
      long: '', 
      lattitude: '',
      longitude: '',
      locfound: false,
      locname: '',
      locweatherdesc: '',
      loctemp: '',
      lochumidity: '',
      icon:'',
      locwind: '',
      flocweatherdesc: [],
      ftemp: [],
      fhumidity: [],
      ficon0: '',
      ficon1: '',
      ficon2: '',
      ficon3: '',
      ficon4: '',
      ficon5: '',
    }
  },
  methods: {
        // getWeather: function(){

        //   var url = `https://cors-anywhere.herokuapp.com/dataservice.accuweather.com/locations/v1/cities/autocomplete?apikey=qhfTcEvsGTNcTMIps5rAD1rPSr7jd6An&q=${this.cityname}`
        //   fetch(url).then(result => {
        //   console.log(result);
        //   return result.json();
        // })
        // .then(data => {
        //   console.log(data)
        //   this.key = data[0].Key
        //   this.cityname = `${data[0].LocalizedName}, ${data[0].AdministrativeArea.LocalizedName}, ${data[0].Country.LocalizedName}`
          
        //   // fetching city weather
        //   fetch(`https://cors-anywhere.herokuapp.com/dataservice.accuweather.com/currentconditions/v1/${this.key}?apikey=qhfTcEvsGTNcTMIps5rAD1rPSr7jd6An`)
        //   .then(result => {
        //     return result.json();
        //   })
        //   .then(data =>{
        //     console.log(data)
        //     this.temp = data[0].Temperature.Metric.Value;
        //     this.ppt = data[0].HasPrecipitation;
        //     if (this.ppt){
        //       this.rain = 'Raining'
        //       this.raintype = data[0].PrecipitationType;
        //     } else {
        //       this.rain = 'Not Raining'
        //     }

        //     this.weatherText = data[0].WeatherText;
        //     this.icon = `https://www.accuweather.com/images/weathericons/0${data[0].WeatherIcon}.svg`

        //   })

        // })

        // this.gotweather = true
            
        // },


        resetWeather: function(){
            this.locfound = false;
        },

        getLocation: function() {
          if (navigator.geolocation) {
             navigator.geolocation.getCurrentPosition(successFunction);
             this.locfound = true;
           } else {
            alert("This browser doesn't support geolocation");
           }
          var vm = this;
          function successFunction(position) {
            vm.lat = position.coords.latitude;
            vm.long = position.coords.longitude;

            vm.lattitude = `Lattitude: ${vm.lat}`
            vm.longitude = `Longitude: ${vm.long}`

            var url = "https://cors-anywhere.herokuapp.com/api.openweathermap.org/data/2.5/weather?lat="+vm.lat+"&lon="+vm.long+"&APPID=84c375472712a1a880ffba2edbc74476&units=metric";
            fetch(url).then(result => {
                  console.log(result);
                  return result.json();
                })
            .then(data => {
             
                  vm.locname = `Location: ${data.name}, ${data.sys.country}`;
                  vm.locweatherdesc = `${data.weather[0].main} | ${data.weather[0].description}`
                  vm.loctemp = `Temp: ${data.main.temp} C | Feels Like: ${data.main.feels_like} C`
                  vm.lochumidity = `Humidity: ${data.main.humidity} %`;
                  vm.locwind = `Wind Speed: ${data.wind.speed} m/s | Wind Direction ${data.wind.deg} deg`
                  vm.icon = `https://openweathermap.org/img/w/${data.weather[0].icon}.png`
            })

            var url2 = "https://cors-anywhere.herokuapp.com/api.openweathermap.org/data/2.5/onecall?lat="+vm.lat+"&lon="+vm.long+"&exclude=hourly&appid=84c375472712a1a880ffba2edbc74476&units=metric";
            fetch(url2).then(result => {
                  console.log(result);
                  return result.json();
                })
            .then(data => {
             console.log(data)
             for (var i = 0; i < 6; i++){

               vm.flocweatherdesc.push(`${data.daily[i].weather[0].main} | ${data.daily[i].weather[0].description}`)
               vm.ftemp.push(`Temp: ${data.daily[i].temp.day} C  (Day), | ${data.daily[i].temp.night} C (Night)`)
               vm.fhumidity.push(`Humidity: ${data.daily[i].humidity} %`)
               
             }

             vm.ficon0 = `https://openweathermap.org/img/w/${data.daily[0].weather[0].icon}.png`
             vm.ficon1 = `https://openweathermap.org/img/w/${data.daily[0].weather[0].icon}.png`
             vm.ficon2 = `https://openweathermap.org/img/w/${data.daily[0].weather[0].icon}.png`
             vm.ficon3 = `https://openweathermap.org/img/w/${data.daily[0].weather[0].icon}.png`
             vm.ficon4 = `https://openweathermap.org/img/w/${data.daily[0].weather[0].icon}.png`
             vm.ficon5 = `https://openweathermap.org/img/w/${data.daily[0].weather[0].icon}.png`
              
            })


          }

         }

    }
}
</script>

<style>
 /*set border to the form*/ 
          
    form { 
        border: 3px solid #f1f1f1; 
    } 
    /*assign full width inputs*/ 
    
    .navbar-header h4{
        margin-left: 20px;
        color: #f1f1f1;
    }
    input[type=text], 
    input[type=password] { 
        width: 60%; 
        padding: 12px 20px; 
        margin: 8px 0; 
        /* display: inline-block;  */
        border: 1px solid #ccc; 
        box-sizing: border-box; 
        border-radius: 5px;;
    } 
    /*set a style for the buttons*/ 
      
    .submit { 
        background-color: #4CAF50; 
        color: white; 
        padding: 14px 20px; 
        margin: 8px 0; 
        border: none; 
        cursor: pointer; 
        width: 40%; 
    } 
    .output {
        background-color: #073d09; 
        color: white; 
        padding: 14px 20px; 
        margin: 8px 0; 
        border-radius: 5px;
        cursor: None; 
        width: 80%;
    }
    /* set a hover effect for the button*/ 
      
    button:hover { 
        opacity: 0.8; 
    } 
    /*set extra style for the cancel button*/ 
      
    .cancelbtn { 
        width: auto; 
        padding: 10px 18px; 
        background-color: #f44336;
        border-radius: 5px; 
    } 
    /*centre the display image inside the container*/ 
      
    .imgcontainer { 
        text-align: center; 
        margin: 24px 0 12px 0; 
    } 
    /*set image properties*/ 
      
    img.avatar { 
        width: 40%; 
        border-radius: 50%; 
    } 
    /*set padding to the container*/ 
      
    .container { 
        padding: 10px; 
        border-radius: 5px;
    } 
    /*set the forgot password text*/ 
      
    span.psw { 
        float: right; 
        padding-top: 5px; 
    } 
    /*set styles for span and cancel button on small screens*/ 
      
    @media screen and (max-width: 300px) { 
        span.psw { 
            display: block; 
            float: none; 
        } 
        .cancelbtn { 
            width: 100%; 
        } 
    } 

    .card {
    background-color: dodgerblue;
    color: white;
    padding: 1rem;
    height: 200px;
    border-radius: 5px;
}

.cards {
  max-width: 1200px;
  margin: 5px auto;
  display: grid;
  grid-gap: 1rem;
}

/* Screen larger than 600px? 2 column */
@media (min-width: 300px) {
  .cards { grid-template-columns: repeat(2, 1fr); }
}

/* Screen larger than 900px? 3 columns */
@media (min-width: 900px) {
  .cards { grid-template-columns: repeat(3, 1fr); }
}

</style>