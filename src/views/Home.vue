<template>
  <div id="home">
    <div class="container">
      <!-- FRONT PAGE / INPUT ZIP -->
      <div v-if="Object.keys(data).length == 0" class="info">
        <h1>Weather App</h1>
        <p>Get your local weather</p>
        <input v-model="searchValue" @keydown.enter="fetchWeather()" type="text" placeholder="search by zip or city">
        <button @click="fetchWeather()">Search</button>
        <p v-if="showResultsError" class="error">There were no results, try a different zip.</p>
      </div>
      <!-- RESULTS -->
      <div v-else class="results">
        <div class="weather-img">
          <img :src="`http://openweathermap.org/img/wn/${data.weather[0].icon}@2x.png`" />
        </div>
        <h5>{{ (data.weather[0].description) }} in your area.</h5>
        <p>{{ data.name }}</p>
        <p class="temperature"> {{ Math.ceil(data.main.temp) }}°</p>
        <button @click="data = {}"> new location</button>
      </div>
    </div>
  </div>
</template>

<script>
  export default {
    data() {
      return {
        searchValue: '',
        data: {},
        showResultsError: false
      }
    },
    watch: {
      searchValue(val) {
        if (val === '') {
          return
        }

        this.showResultsError = false
      }
    },
    methods: {
      // FETCH API DATA
      async fetchWeather() {
        try {
          const apiKey = 'a38a8ac08913133d21521e824b908cb5'
          let api = `https://api.openweathermap.org/data/2.5/weather?zip=${this.searchValue},us&units=imperial&appid=${apiKey}`

          // USE CITY INSTEAD OF ZIP
          if (isNaN(this.searchValue)) {
            api = `https://api.openweathermap.org/data/2.5/weather?q=${this.searchValue},us&units=imperial&appid=${apiKey}`
          }

          // AWAIT RESPONSE
          let res = await fetch(api)
          let data = await res.json()
          // IF THERE IS DATA
          if (data.main) {
            this.data = data
          } else {
            this.showResultsError = true
          }
          // RESET SEARCH FEILD
          this.searchValue = ''

        } catch (err) {
          alert('network error, try again')
        }
      }
    }
  }
</script>

<style lang="scss" scoped>
  #home {
    display: flex;
    height: 100vh;
  }

  .error {
    position: absolute;
    bottom: -75px;
    left: 0;
    right: 0;
    color: $error;
  }

  .container {
    max-width: 800px;
    margin: auto;
    display: flex;
    flex-direction: column;
  }

  .info {
    position: relative;
    text-align: center;

    h1 {

    }
    p {
      margin: 10px 0 20px;
    }
    input {
      width: 100%;
    }
    button {
      margin-top: 15px;
      width: 100%;
    }
  }

  .results {
    display: flex;
    text-align: center;
    flex-direction: column;
    padding: 15px 0;

    .temperature {
        font-size: 45px;
    }

    h4 {}
  }
</style>
