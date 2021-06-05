<template>
  <div class="container">
    <div class="inner-container">
      <h1 class="title">
        Autoilumittari
      </h1>
      <Progress :state="state" @stateChange="stateChange" />
      <Cars v-if=" state.active === 1 " :selected="selection.car" @carClick="saveCar" />
      <Distance
        v-if=" state.active === 2 "
        :selected="selection.distance"
        @distance="saveDistance"
      />
      <Speed v-if=" state.active === 3" :selected="selection.speeds" @speed="saveSpeed" />
      <Results v-if=" state.active == 4 " :selection="selection" />
    </div>
    <a id="attribution" href="https://www.vecteezy.com/free-vector/car">Car Vectors by Vecteezy</a>
  </div>
</template>

<script>
export default {
  data: function() {
    return {
      state: {
        active: 1,
        completed: []
      },
      selection: {
        car: "",
        img: "",
        consumption: null,
        distance: null,
        speeds: [null, null]
      }
    }
  },
  methods: {
    stateChange: function(value) {
      this.state.active = value
    },
    saveCar: function(value) {
      this.selection.car = value[0]
      this.selection.img = value[1]
      this.selection.consumption = value[2]
      this.state.completed.push(1)
      this.stateChange(2)
    },
    saveDistance: function(value) {
      this.selection.distance = value
      this.state.completed.push(2)
      this.stateChange(3)
    },
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
