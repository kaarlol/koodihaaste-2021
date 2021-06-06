<template>
  <div class="container">
    <div class="inner-container">
      <h1 class="title">
        Autoilumittari
      </h1>
      <!-- Progress bar and H2 title component of the app. Gets current state as a prop and emits state changes to parent -->
      <Progress :state="state" @stateChange="stateChange" />
      <!-- Car selection component. Shown on the first stage. Get's car selection as a prop if already defined and emits selected car to parent -->
      <Cars v-if=" state.active === 1 " :selected="selection.car" @carClick="saveCar" />
      <!-- Distance selection component. Shown on the second stage. Get's distance input as a prop if already defined and emits set input value to parent -->
      <Distance
        v-if=" state.active === 2 "
        :selected="selection.distance"
        @distance="saveDistance"
      />
      <!-- Speeds selection component. Shown on the third stage. Get's speed selection as a prop if already defined and emits inputed speeds to parent -->
      <Speed v-if=" state.active === 3" :selected="selection.speeds" @speed="saveSpeed" />
      <!-- Result selection component. Shown on the fourth and last stage. Get's all earlier selections as a prop  -->
      <Results v-if=" state.active == 4 " :selection="selection" />
    </div>
    <!-- Just an attribution for used vector images, since it's not cool to steal stuff -->
    <a id="attribution" href="https://www.vecteezy.com/free-vector/car">Car Vectors by Vecteezy</a>
  </div>
</template>

<script>
export default {
  data: function() {
    return {
      state: {
        active: 1, // Define which state is active on progress bar
        completed: [] // Remember which stages have been completeds
      },
      selection: {
        car: "", // Save car's id
        img: "", // Save car's image
        consumption: null, // Save car's base consumption
        distance: null, // Save distance to location
        speeds: [null, null] // Save speeds to compare
      }
    }
  },
  methods: {
    // Method to change the state
    stateChange: function(value) {
      this.state.active = value
    },
    // Method to save car selection
    saveCar: function(value) {
      this.selection.car = value[0]
      this.selection.img = value[1]
      this.selection.consumption = value[2]
      this.state.completed.push(1)
      this.stateChange(2)
    },
    // Method to save distance selection
    saveDistance: function(value) {
      this.selection.distance = value
      this.state.completed.push(2)
      this.stateChange(3)
    },
    // Method to save speeds to compare
    saveSpeed: function(value) {
      this.selection.speeds = value
      this.state.completed.push(3, 4)
      this.stateChange(4)
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
}

.title {
  font-family: Roboto;
  font-weight: 400;
  font-size: 3rem;
}

#attribution {
    position: absolute;
    bottom: 1rem;
    left: 50%;
    transform: translateX(-50%);
    font-size: 0.8rem;
  }

@media only screen and (max-width: 977px) {
  #attribution {
    position: relative;
  }
}
</style>
