<template>
  <div id="home">
    <div class="container">
      <div v-if="Object.keys(data).length == 0" class="info">
        <h1>Weather App</h1>
        <p>Get your local wether</p>
        <input v-model="searchValue" @keydown.enter="fetchWeather()" type="text" placeholder="search by zip">
        <button @click="fetchWeather()" >Search</button>
        <p v-if="showResultsError" class="error">There were no results, try a differnt zip.</p>
      </div>
      <div v-else class="results">
        <p>the weather is: {{ Math.ceil(data.main.temp) }} degrees</p>
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
      async fetchWeather() {
        try {
          const apiKey = 'a38a8ac08913133d21521e824b908cb5'
          const api = `https://api.openweathermap.org/data/2.5/weather?zip=${this.searchValue},us&units=imperial&appid=${apiKey}`

          let res = await fetch(api)
          let data = await res.json()

          if (data.main) {
            this.data = data
          } else {
            this.showResultsError = true
          }

          this.searchValue = ''
        } catch (err) {
          console.log(err);
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
    padding: 20px 0;

    p {

    }
  }
</style>
