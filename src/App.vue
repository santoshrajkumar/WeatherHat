<template>

  <div>
    <center>
    <div class="container">
      <button @click="getLocation" class="btn btn-primary">Get Your Location Data</button>
      <br><br>
      <div class="container" style="background-color:#f1f1f1" v-if="locfound">
        <h4>Lattitude of your location: {{ lat }}</h4>
        <h4 style="color=#fff">Longitude of your location: {{ long }}</h4>
      </div>
    </div>
    </center>

      <center>
        <h4>Enter name of any location (keep no space) to know the current weather</h4>
        
      <div class="container" @keyup.enter="getWeather"> 
      
            <label>Enter Name of The Place: </label>
            <input type="text" placeholder="Type here" v-model="cityname" required> <br> 
            <br><br>
        
        <button class="btn btn-primary" v-on:click="getWeather">Submit</button>
        <br>
      </div> 
      </center>

      <div class="container" style="background-color:#f1f1f1" v-if="gotweather"> 
            <img v-bind:src="icon" alt="">
            <p>Location {{ cityname }}</p>
            <p>{{ weatherText }}</p>
            <p>Temperature: {{ temp }} deg C</p>
            <p>{{ rain }}</p>
            <p v-if="ppt"> Precipitation Type: {{ raintype }} </p>
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
      cityname: '',
      key: '',
      temp: '',
      weatherText: '',
      ppt: false,
      rain: '',
      raintype: '',
      icon: '',
      gotweather: false,
      lat: '',
      long: '', 
      locfound: false
    }
  },
  methods: {
        getWeather: function(){

          var url = `https://cors-anywhere.herokuapp.com/dataservice.accuweather.com/locations/v1/cities/autocomplete?apikey=qhfTcEvsGTNcTMIps5rAD1rPSr7jd6An&q=${this.cityname}`
          fetch(url).then(result => {
          console.log(result);
          return result.json();
        })
        .then(data => {
          console.log(data)
          this.key = data[0].Key
          this.cityname = `${data[0].LocalizedName}, ${data[0].AdministrativeArea.LocalizedName}, ${data[0].Country.LocalizedName}`
          
          // fetching city weather
          fetch(`https://cors-anywhere.herokuapp.com/dataservice.accuweather.com/currentconditions/v1/${this.key}?apikey=qhfTcEvsGTNcTMIps5rAD1rPSr7jd6An`)
          .then(result => {
            return result.json();
          })
          .then(data =>{
            console.log(data)
            this.temp = data[0].Temperature.Metric.Value;
            this.ppt = data[0].HasPrecipitation;
            if (this.ppt){
              this.rain = 'Raining'
              this.raintype = data[0].PrecipitationType;
            } else {
              this.rain = 'Not Raining'
            }

            this.weatherText = data[0].WeatherText;
            this.icon = `https://www.accuweather.com/images/weathericons/0${data[0].WeatherIcon}.svg`

          })

        })

        this.gotweather = true
            
        },


        resetWeather: function(){
            this.cityname = '',
            this.key = '',
            this.gotweather = false
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
        }

        var url = `https://cors-anywhere.herokuapp.com/dataservice.accuweather.com/locations/v1/cities/autocomplete?apikey=qhfTcEvsGTNcTMIps5rAD1rPSr7jd6An&q=${this.cityname}`
          fetch(url).then(result => {
          console.log(result);
          return result.json();
        })
        .then(data => {
          console.log(data)
        })


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
</style>