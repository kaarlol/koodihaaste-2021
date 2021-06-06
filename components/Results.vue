<template>
  <div class="results">
    <!-- Small ingress to remind about earlier car and distance selections -->
    <p
      class="strong"
    >
      Olet liikkeellä autolla {{ selection.car }} ja sinulla on {{ selection.distance }} km ajettavana.
    </p>
    <!-- Table view to compare results with two different speeds -->
    <table>
      <thead>
        <tr>
          <th />
          <th class="strong">
            Nopeus 1: {{ selection.speeds[0] }} km/h
          </th>
          <th class="strong">
            Nopeus 2: {{ selection.speeds[1] }} km/h
          </th>
          <th class="strong">
            Tulokset
          </th>
        </tr>
      </thead>
      <tbody>
        <tr>
          <td class="strong">
            Matka-aika
          </td>
          <!-- Speed 1 travel time -->
          <td>{{ secondsToHms( times[0] ) }}</td>
          <!-- Speed 2 travel time -->
          <td>{{ secondsToHms( times[1] ) }}</td>
          <!-- Show which speed is faster -->
          <td>{{ resultTime }}</td>
        </tr>
        <tr>
          <td class="strong">
            Kulutus
          </td>
          <!-- Speed 1 consumption -->
          <td>{{ consumptions[0].toFixed(2) }} litraa</td>
          <!-- Speed 2 consumption -->
          <td>{{ consumptions[1].toFixed(2) }} litraa</td>
          <!-- Show which speed has smaller consumption -->
          <td>{{ resultConsumption }}</td>
        </tr>
      </tbody>
    </table>
  </div>
</template>

<script>
export default {
  props: {
    selection: { // Get earlier selections as prop from parent page
      type: Object,
      default: function() {
        return {}
      }
    }
  },
  computed: {
    // Computed property for travel times
    times: function() {
      return [
        this.calculateTime(this.selection.speeds[0], this.selection.distance),
        this.calculateTime(this.selection.speeds[1], this.selection.distance)
      ]
    },
    // Computed property that returns a comparison between travel times
    resultTime: function() {
      if (this.times[0] < this.times[1]) {
        return (
          "Nopeudella 1 olet perillä " +
          this.secondsToHms(this.times[1] - this.times[0]) +
          " aiemmin"
        )
      } else if (this.times[1] < this.times[0]) {
        return (
          "Nopeudella 2 olet perillä " +
          this.secondsToHms(this.times[0] - this.times[1]) +
          " aiemmin"
        )
      } else {
        return "Matka-aika on sama, koska valitsit samat nopeudet."
      }
    },
    // Computed property for consumptions
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
    // Computed property that returns a comparison between consumptions
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
    // Method to convert resulting hours tto seconds for comparison
    hoursToSeconds: function(hours) {
      let seconds = Math.round(hours * 3600)
      return seconds
    },
    // Method to convert seconds to more human readable time format
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
    // Method to calculate travel time based on speed and distance
    calculateTime: function(speed, distance) {
      let hours = distance / speed
      return this.hoursToSeconds(hours)
    },
    // Method to calculate consumption based on base consumption, speed and distance with given rate of change (1.009)
    calculateConsumption: function(consumption, speed, distance) {
      let consumptionPerKm = (consumption * 1.009 ** (speed - 1)) / 100
      let totalConsumption = consumptionPerKm * distance
      return totalConsumption
    }
  }
}
</script>

<style scoped>
.results {
  display: flex;
  align-items: center;
  margin: 2rem;
  justify-content: center;
  flex-direction: column;
}

table {
  text-align: left;
  border-collapse: collapse;
}

thead tr th {
  border-bottom: 2px solid black;
}

th,
td {
  padding: 1rem 3rem 0.5rem 0;
}

th {
  vertical-align: bottom;
}

td {
  vertical-align: top;
  max-width: 15rem;
}

.strong {
  font-weight: 500;
}

@media only screen and (max-width: 600px) {
    .results {
        margin: 2rem 0.5rem 3rem 0.5rem;
    }
    table {
        font-size: 0.8em;
    }
    th,
    td {
        padding: 0.5rem 1rem 0.5rem 0;
    }
}

@media only screen and (max-width: 320px) {
    table {
        font-size: 0.7em;
    }
}
</style>