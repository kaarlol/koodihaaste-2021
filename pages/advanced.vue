<template>
  <div class="container">
    <h1 class="title">
      Autoilumittari v2.0
    </h1>
    <h2>Valitse auto</h2>
    <div class="cars">
      <div
        v-for="car in cars"
        :key="car.id"
        class="car"
        :class="selectedCar(car.id)"
        @click="carClick(car.id, car.consumption)"
      >
        <img :src="car.img" :alt="'Auto ' + car.id">
        <h2>Auto {{ car.id }}</h2>
        <p>
          Kulutus {{ car.consumption }}l / 100km
          <br>1km/h nopeudella
        </p>
      </div>
    </div>
    <h2>Määritä välimatka</h2>
    <p>{{ selection.distance }} kilometriä</p>
    <range-slider v-model="selection.distance" class="slider" min="1" max="1000" step="1" />
    <h2>Määritä vertailtavat nopeudet</h2>
    <p>{{ selection.speeds[0] }} vs {{ selection.speeds[1] }} km/h</p>
    <h3>Nopeus 1</h3>
    <range-slider v-model="selection.speeds[0]" class="slider" min="1" max="500" step="1" />
    <h3>Nopeus 2</h3>
    <range-slider v-model="selection.speeds[1]" class="slider" min="1" max="500" step="1" />
    <h2>Tulokset</h2>
    <p class="results">
      {{ results }}
    </p>
    <a id="attribution" href="https://www.vecteezy.com/free-vector/car">Car Vectors by Vecteezy</a>
  </div>
</template>

<script>
import RangeSlider from "vue-range-slider"
import "vue-range-slider/dist/vue-range-slider.css"

export default {
  components: {
    RangeSlider
  },
  data: function() {
    return {
      selection: {
        car: "A",
        consumption: 3,
        distance: 1,
        speeds: [1, 1]
      },
      cars: [
        {
          id: "A",
          img: require("~/assets/auto-a.svg"),
          consumption: 3
        },
        {
          id: "B",
          img: require("~/assets/auto-b.svg"),
          consumption: 3.5
        },
        {
          id: "C",
          img: require("~/assets/auto-c.svg"),
          consumption: 4
        }
      ]
    }
  },
  computed: {
    times: function() {
      return [
        this.calculateTime(this.selection.speeds[0], this.selection.distance),
        this.calculateTime(this.selection.speeds[1], this.selection.distance)
      ]
    },
    results: function() {
      if (this.times[0] < this.times[1]) {
        return (
          "Nopeudella 1 olet perillä " +
          this.secondsToHms(this.times[1] - this.times[0]) +
          " aiemmin, mutta olet kuluttanut " +
          (this.consumptions[0] - this.consumptions[1]).toFixed(2) +
          " litraa enemmän polttoainetta."
        )
      } else if (this.times[1] < this.times[0]) {
        return (
          "Nopeudella 2 olet perillä " +
          this.secondsToHms(this.times[0] - this.times[1]) +
          " aiemmin, mutta olet kuluttanut " +
          (this.consumptions[1] - this.consumptions[0]).toFixed(2) +
          " litraa enemmän polttoainetta."
        )
      } else {
        return "Matka-aika ja polttoaineen kulutus ovat samat, koska valitsit samat nopeudet."
      }
    },
    consumptions: function() {
      return [
        this.calculateConsumption(
          this.selection.consumption,
          this.selection.speeds[0],
          this.selection.distance
        ),
        this.calculateConsumption(
          this.selection.consumption,
          this.selection.speeds[1],
          this.selection.distance
        )
      ]
    },
    resultConsumption: function() {
      if (this.consumptions[0] < this.consumptions[1]) {
        return (
          "Nopeudella 1 kulutuksesi on " +
          (this.consumptions[1] - this.consumptions[0]).toFixed(2) +
          " litraa matalampi"
        )
      } else if (this.consumptions[1] < this.consumptions[0]) {
        return (
          "Nopeudella 2 kulutuksesi on " +
          (this.consumptions[0] - this.consumptions[1]).toFixed(2) +
          " litraa matalampi"
        )
      } else {
        return "Kulutus on sama, koska valitsit samat nopeudet."
      }
    }
  },
  methods: {
    selectedCar: function(id) {
      return this.selection.car === id ? "selected" : ""
    },
    carClick: function(id, consumption) {
      this.selection.car = id
      this.selection.consumption = consumption
    },
    hoursToSeconds: function(hours) {
      let seconds = Math.round(hours * 3600)
      return seconds
    },
    secondsToHms: function(seconds) {
      let h = Math.floor(seconds / 3600)
      let m = Math.floor((seconds % 3600) / 60)
      let s = Math.floor((seconds % 3600) % 60)

      function addPlural(value) {
        return value > 1 ? "a" : ""
      }

      let result = h === 0 ? "" : h + " tunti" + addPlural(h) + " "
      result += m === 0 ? "" : m + " minuutti" + addPlural(m) + " "
      result += s === 0 ? "" : s + " sekunti" + addPlural(s)
      return result
    },
    calculateTime: function(speed, distance) {
      let hours = distance / speed
      return this.hoursToSeconds(hours)
    },
    calculateConsumption: function(consumption, speed, distance) {
      let consumptionPerKm = (consumption * 1.009 ** (speed - 1)) / 100
      let totalConsumption = consumptionPerKm * distance
      return totalConsumption
    }
  }
}
</script>

<style>
.container {
  margin: 0 auto;
  display: flex;
  justify-content: center;
  text-align: center;
  flex-direction: column;
  max-width: 50rem;
  padding: 0 2rem;
}

.title {
  font-family: Roboto;
  font-weight: 400;
  font-size: 2.5rem;
}

.cars {
  display: flex;
  align-items: center;
  justify-content: center;
  flex-wrap: wrap;
}

.car {
  margin: 1rem;
  padding: 2rem;
  height: 12rem;
  display: flex;
  justify-content: flex-end;
  align-items: center;
  flex-direction: column;
  border-radius: 0.5rem;
}

.car.selected {
  background: #eee;
}

.car:hover {
  background: #eee;
  cursor: pointer;
}

.slider {
  width: 100%;
  margin: auto;
}

.slider .range-slider-fill {
  background-color: #777;
}

p.results {
    margin-bottom: 10rem;
}
</style>