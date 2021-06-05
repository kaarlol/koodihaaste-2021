<template>
  <div class="wrapper">
    <div class="speeds">
      <div class="speed">
        <p class="errors">
          {{ errorOne }} &nbsp;
        </p>
        <input v-model.number="speedOne" type="number" min="1" placeholder="Syötä nopeus 1">
      </div>
      <div class="speed">
        <p class="errors">
          {{ errorTwo }} &nbsp;
        </p>
        <input v-model.number="speedTwo" type="number" min="1" placeholder="Syötä nopeus 2">
      </div>
    </div>
    <button @click="emitSpeed">
      Tallenna vertailtavat nopeudet
    </button>
  </div>
</template>

<script>
export default {
    props: {
      selected: {
        type: Array,
        default: function() {
          return [null, null]
        }
      }
    },
    data: function() {
        return {
            speedOne: this.selected[0],
            speedTwo: this.selected[1],
            errorOne: '',
            errorTwo: ''
        }
    }, 
    methods: {
        emitSpeed: function() {
            let errors = true

            this.errorOne = ''
            this.errorTwo = ''

            if (this.speedOne === null) {
                this.errorOne = 'Syötä nopeus.'
            } else if (this.speedOne === 0) {
                this.errorOne = 'Antamasi nopeus ei voi olla 0 km/h.'
            } else if (this.speedOne < 0) {
                this.errorOne = 'Antamasi nopeus ei voi olla negatiivinen.'
            } else {
                errors = false
            }
            
            if (this.speedTwo === null) {
                this.errorTwo = 'Syötä nopeus.'
            } else if (this.speedTwo === 0) {
                this.errorTwo = 'Antamasi nopeus ei voi olla 0 km/h.'
            } else if (this.speedTwo < 0) {
                this.errorTwo = 'Antamasi nopeus ei voi olla negatiivinen.'
            } else {
                errors = false
            }

            if (!errors) {
                this.$emit('speed', [this.speedOne, this.speedTwo])
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