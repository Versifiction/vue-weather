<template
  ><div>
    <div id="weather">
      <div id="weather-content">
        <div id="weather-subcontent">
          <div class="flex temp align">
            <span>{{ temp | roundedInt }}</span>
            <span class="celsius">°C</span>
          </div>
          <p class="city align">{{ city_name }}, {{ country_code }}</p>
          <p class="description align">{{ description }}</p>
          <img class="weather__icon" :src="getImage(icon)" :alt="description" />
          <p class="date align">
            Dernière actualisation :<br />
            {{ date | moment }}
          </p>
        </div>
      </div>
    </div>
    <div id="copyright" class="align">
      © <span class="bold">Marc Charpentier</span> - Un projet VueJS
    </div>
  </div>
</template>

<script>
import axios from "axios";
import moment from "moment";

export default {
  name: "Weather",
  props: {},
  data() {
    return {
      city_name: null,
      date: null,
      description: null,
      icon: null,
      temp: null,
    };
  },
  filters: {
    capitalize: function(value) {
      return value.toUpperCase();
    },
    moment: function(date) {
      return moment(date).format("DD/MM/YYYY - HH:MM");
    },
    roundedInt: function(value) {
      return Math.round(value);
    },
  },
  methods: {
    getImage(icon) {
      return `https://www.weatherbit.io/static/img/icons/${icon}.png`;
    },
  },
  mounted() {
    navigator.geolocation.getCurrentPosition((position) => {
      axios
        .get(
          `${process.env.VUE_APP_API_BASE_URL}?lat=${position.coords.latitude}&lon=${position.coords.longitude}&key=${process.env.VUE_APP_API_KEY}`
        )
        .then((response) => {
          console.log("response :", response);
          this.city_name = response.data.data[0].city_name;
          this.country_code = response.data.data[0].country_code;
          this.description = response.data.data[0].weather.description;
          this.icon = response.data.data[0].weather.icon;
          this.date = response.data.data[0].last_ob_time;
          this.temp = response.data.data[0].app_temp;
        })
        .catch((err) => console.warn(err));
    });
  },
};
</script>

<style>
#weather {
  width: 100%;
  height: 100vh;
  display: flex;
  justify-content: center;
  align-items: center;
}

#weather-content {
  width: 330px;
  height: 600px;
  border-radius: 5px;
  box-shadow: 0 19px 38px rgba(0, 0, 0, 0.3), 0 15px 12px rgba(0, 0, 0, 0.22);
  background-image: url("../assets/images/day.jpg");
  background-position: bottom;
  background-size: cover;
  background-repeat: no-repeat;
  display: flex;
  align-items: center;
  justify-content: center;
}

p {
  color: white;
  margin-block-start: 0;
  margin-block-end: 0;
}

.flex {
  display: flex;
  justify-content: center;
  align-items: center;
}

.align {
  text-align: center;
}

.date {
  font-size: 0.75rem;
}

.temp {
  color: white;
  font-size: 4rem;
}

.celsius {
  font-size: 1rem;
}

.city {
  font-weight: bold;
  font-size: 1.5rem;
}

.description {
  font-size: 1rem;
}

.weather__icon {
  display: block;
  margin: 0 auto;
}

#copyright {
  color: white;
  position: absolute;
  bottom: 0;
  left: 50%;
  transform: translate(-50%);
}

.bold {
  font-weight: bold;
}
</style>
