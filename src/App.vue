<template>
    <div class="weather" :class="weatherClass">
        <div class="container">
            <div class="card weather-form">
                <input type="text" class="weather-form__input" placeholder="Enter city" @keyup.enter="weatherSearch" v-model="searchQuery">
                <button class="weather-form__btn" @click="weatherSearch">Search</button>
            </div>
            <div class="card weather-load" v-if="loading">Loading...</div>

            <div class="weather-info" v-show="!error && location && temp !== 0 && desc">
                <div class="card" v-if="error">Error</div>

                <div class="weather-info__text">
                    <p class="card">{{location}}</p>
                    <p class="card">{{temp}}°С</p>
                    <p class="card">{{desc}}</p>
                </div>
            </div>
        </div>

        <div class="weather-bg">
            <div>
                <img src="./assets/default.jpg" alt="bg" class="weather-bg__img bg">
                <img src="./assets/rain.jpg" alt="overcast" class="weather-bg__img overcast">
                <img src="./assets/cloudly.jpg" alt="cloudy" class="weather-bg__img partly-cloudy">
                <img src="./assets/sunny.jpg" alt="sunny" class="weather-bg__img sunny">
            </div>

        </div>
    </div>
</template>

<script>
export default {
    data() {
        return {
            location: '',
            temp: 0,
            desc: '',
            loading: false,
            error: false,
            searchQuery: '',

        };
    },
    computed: {
        weatherClass() {
            if (this.desc.includes('Sunny')) {
                return 'sunny';
            } else if (this.desc.includes('Overcast')) {
                return 'overcast';
            } else if (this.desc.includes('Partly cloudy')) {
                return 'partly-cloudy';
            } else {
                return '';
            }
        }

    },
    methods: {
        weatherSearch() {
            this.loading = true;
            this.error = false;
            try {
                fetch(`https://api.weatherapi.com/v1/current.json?key=2698e8ab1a2c41cbb2b91155240105&q=${this.searchQuery}`)
                .then(response => response.json())
                .then(data => {
                    this.loading = false;
                    this.location = data.location.name;
                    this.temp = data.current.temp_c;
                    this.desc = data.current.condition.text;
                    this.resetSearchQuery();
                })
            } catch (error) {
                this.loading = false;
                this.error = true;
                console.error(error);
            }
            

        },
        resetSearchQuery() {
            this.searchQuery = '';
        }
    }
};
</script>
