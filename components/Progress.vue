<template>
  <div class="wrapper">
    <div class="steps">
      <!-- Loop through steps array in data to set html content -->
      <div
        v-for="step in steps"
        :key="step.number"
        class="step"
        :class="[getActiveClass(step.number),getCompletedClass(step.number)]"
      >
        <div class="circle" @click="linkBack(step.number)">
          {{ step.number }}
        </div>
        <p>{{ step.title }}</p>
      </div>
    </div>
    <h2>{{ steps[state.active-1].text }}</h2>
  </div>
</template>

<script>
export default {
  props: {
    state: { // Receive state from parent page (index.vue)
      type: Object,
      default: function() {
        return {}
      }
    }
  },
  data: function() {
    return {
      steps: [ // Define content for the progress bar and stage titles
        {
          number: 1,
          title: "Auto",
          text: "Valitse auto, jolla haluat matkustaa"
        },
        {
          number: 2,
          title: "Matka",
          text: "Syötä matka kilometreissä"
        },
        {
          number: 3,
          title: "Nopeus",
          text: "Anna nopeudet (km/h), joita haluat vertailla"
        },
        {
          number: 4,
          title: "Tulokset",
          text: "Vertaile tuloksia"
        }
      ]
    }
  },
  methods: {
    // Add css class active to currently active stage
    getActiveClass: function(value) {
      return this.state.active === value ? "active" : ""
    },
    // Add css class completed to those stages that have been completed
    getCompletedClass: function(value) {
      return this.state.completed.includes(value) ? "completed" : ""
    },
    // Allow to navigate back to those stages that have been set already
    linkBack: function(value) {
      this.state.completed.includes(value)
        ? this.$emit("stateChange", value)
        : ""
    }
  }
}
</script>

<style scoped>
.wrapper {
  max-width: 36rem;
  margin: auto;
}

.steps {
  display: flex;
  justify-content: space-around;
}

.step {
  margin: 0.5rem 1rem;
  color: #aaa;
}

.step.active {
  color: #000;
}

.circle {
  border: 1px solid #aaa;
  border-radius: 50%;
  width: 3rem;
  height: 3rem;
  display: flex;
  justify-content: center;
  align-items: center;
  font-weight: 400;
  margin: auto;
}

.completed .circle {
  border: 1px solid #aaa;
  background-color: #aaa;
  color: #fff;
  cursor: pointer;
}

.active .circle {
  border: 1px solid #000;
  background-color: #000;
  color: #fff;
}

.step p {
  text-align: center;
  max-width: 10rem;
}
</style>
