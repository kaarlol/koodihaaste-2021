<template>
  <div class="wrapper">
    <div class="speeds">
      <div class="speed">
        <!-- Show input field validation errors -->
        <p class="errors">
          {{ errorOne }} &nbsp;
        </p>
        <!-- An input field to set the first speed to compare -->
        <input v-model.number="speedOne" type="number" min="1" placeholder="Syötä nopeus 1">
      </div>
      <div class="speed">
        <!-- Show input field validation errors -->
        <p class="errors">
          {{ errorTwo }} &nbsp;
        </p>
        <!-- An input field to set the second speed to compare -->
        <input v-model.number="speedTwo" type="number" min="1" placeholder="Syötä nopeus 2">
      </div>
    </div>
    <!-- Button to save the speeds and emit them to parent -->
    <button @click="emitSpeed">
      Tallenna vertailtavat nopeudet
    </button>
  </div>
</template>

<script>
export default {
    props: {
      selected: { // Get earlier set speeds from parent
        type: Array,
        default: function() {
          return [null, null]
        }
      }
    },
    data: function() {
        return {
            speedOne: this.selected[0], // Set the default speed 1 to that gotten from parent
            speedTwo: this.selected[1], // Set the default speed 2 to that gotten from parent
            errorOne: '', // First validation error value placeholder
            errorTwo: '' // Second validation error value placeholder
        }
    }, 
    methods: {
        // Method to emit input values to parent for saving
        emitSpeed: function() {
            this.errorOne = ''
            this.errorTwo = ''

            if (this.validateInput('speedOne', 'errorOne') && this.validateInput('speedTwo', 'errorTwo')) {
              this.$emit('speed', [this.speedOne, this.speedTwo])
            }

        },
        // Method to validate the input values used inside emitSpeed method
        validateInput: function(speed, error) {
          if (this[speed] === null) {
                this[error] = 'Syötä nopeus.'
            } else if (this[speed] === 0) {
                this.[error] = 'Antamasi nopeus ei voi olla 0 km/h.'
            } else if (this[speed]< 0) {
                this.[error] = 'Antamasi nopeus ei voi olla negatiivinen.'
            } else {
                return true
            }
        }
    }
}
</script>

<style scoped>
.wrapper {
    display: flex;
    align-items:center;
    margin: 2rem;
    justify-content: center;
    flex-direction: column;
}

.speeds {
    display: flex;
    flex-wrap: wrap;
    justify-content: center;
}

.errors {
    text-align: center;
}

.errors span {
    visibility: hidden;
}

.errors span.visible{
    visibility: initial;
}

input {
    height: 3rem;
    padding: 0.5rem;
    font-size: 1.75rem;
    text-align: center;
    border: 1px solid #000;
    border-radius: 0.5rem;
    margin: 0 1rem;
}

input::placeholder { /* Chrome, Firefox, Opera, Safari 10.1+ */
  color: #aaa;
}

input:-ms-input-placeholder { /* Internet Explorer 10-11 */
  color: #aaa;
}

input::-ms-input-placeholder { /* Microsoft Edge */
  color: #aaa;
}

button {
    margin: 4rem;
    padding: 1rem 1.5rem;
    font-size: 1rem;
    background-color: #000;
    border: none;
    border-radius: 0.5rem;
    color: #fff;
}

button:hover {
    cursor: pointer;
}
</style>