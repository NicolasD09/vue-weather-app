<template>
    <div id="app" :class="(weather &&  weather.main.temp > 60) ? 'hot': ''">
        <h1 class="text-center title mx-auto">Weather app</h1>
        <div class="meteo_container w-25 mx-auto mt-5">
            <div class="form-group w-75 my-auto mx-auto">
                <label for="request">What's your city ?</label>
                <input type="text" class="form-control" name="request" id="request" v-model="request"
                       @keypress.enter="fetchWeather">
            </div>
            <div class="weather_container mx-auto mt-2" v-if="weather">
                <div class="card">
                    <div class="card-body">
                        <h5 class="card-title mx-auto">{{ ucFirst(weather.name) }}</h5>
                        <div class="img-container mx-auto my-1">
                            <img :src="image" alt="weather icon" class="mx-auto my-3">
                        </div>
                        <p class="card-text mx-auto">{{ weather.main.temp.toFixed() }}°F</p>
                        <p class="card-text mx-auto">{{ ucFirst(weather.weather[0].description)
                            }}</p>
                    </div>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
    import axios from 'axios';


    export default {
        name: 'App',
        data() {
            return {
                request: '',
                weather: undefined,
                api_code: process.env.VUE_APP_API_KEY,
                search_url: 'https://api.openweathermap.org/data/2.5/weather?'
            }
        },
        methods: {
            fetchWeather() {
                axios.get(`${this.search_url}q=${this.request}&units=imperial&appid=${this.api_code}&lang=en`)
                    .then(response => {
                        this.weather = response.data;
                        this.image = `http://openweathermap.org/img/wn/${this.weather.weather[0].icon}@2x.png`
                    });
                this.request = '';
            },
            ucFirst(string) {
                return string.charAt(0).toUpperCase() + string.slice(1);
            }
        }
    }
</script>

<style>

    #app {
        position: absolute;
        width: 100vw;
        height: 100vh;
        top: 0;
        left: 0;
        right: 0;
        bottom: 0;
        background-color: #2c3e50;
        background-image: url("./assets/hot.jpg");
        background-size: cover;
        background-position: top center;
    }

    #app.cold {
        background-image: url("./assets/cold.png");
    }

    body {
        padding: 0;
        margin: 0;
        box-sizing: border-box;
        overflow: hidden;
    }

    label {
        color: #f8f8f8;
        font-weight: bold;
        font-size: 1.5rem;
        text-shadow: -1px 0 8px rgba(0, 0, 0, 0.5);
    }

    input {
        background-color: rgba(255, 255, 255, .5) !important;
        border: none !important;
    }

    .form-group {
        background-color: rgba(255, 255, 255, .4);
        padding: 1rem 2rem;
        border-radius: 15px;
    }

    .title {
        color: #f8f8f8;
        margin-top: 4%;
        font-size: 4rem;
        padding: 1.3rem;
        border: 2px solid #f8f9fa;
        width: fit-content;
        border-radius: 15px;
        text-shadow: -4px 4px 0 rgba(0, 0, 0, 0.5);

    }

    .card-title {
        width: fit-content;
        font-size: 2rem;
    }

    .card {
        min-height: 400px;
        background-color: transparent !important;
        border: none !important;
    }

    .card-body {
        display: flex;
        flex-direction: column;
        align-content: center;
    }

    .card-text,
    .card-title {
        font-size: 3rem;
        color: #f8f8f8;
        font-weight: bold;
        text-shadow: -2px 3px 0 rgb(0, 0, 0);

    }

    p {
        width: fit-content;
        font-size: 1.4rem;
    }
</style>
