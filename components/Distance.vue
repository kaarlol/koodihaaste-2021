<template>
  <div class="distance">
    <p class="errors">
      {{ error }} &nbsp;
    </p>
    <input v-model.number="distance" type="number" min="1" placeholder="Anna ajomatka" autofocus @keyup.enter="emitDistance(distance)">
    <button @click="emitDistance">
      Tallenna matkan pituus
    </button>
  </div>
</template>

<script>
export default {
    props: {
      selected: {
        type: Number,
        default: null
      }
    },
    data: function() {
        return {
            distance: this.selected,
            error: ''
        }
    }, 
    methods: {
        emitDistance: function() {
            this.error = ''
            if (this.distance === null) {
                this.error = 'Syötä matkan pituus.'
            } else if (this.distance === 0) {
                this.error = 'Antamasi matkan pituus ei voi olla 0 km.'
            } else if (this.distance < 0) {
                this.error = 'Antamasi matkan pituus ei voi olla negatiivinen.'
            } else {
                this.error = ''
                this.$emit('distance', this.distance)
            }
            
        }
    }
}
</script>

<style scoped>
.distance {
    display: flex;
    align-items:center;
    margin: 2rem;
    justify-content: center;
    flex-direction: column;
}

.errors {
    text-align: center;
}

input {
    height: 3rem;
    padding: 0.5rem;
    font-size: 1.75rem;
    text-align: center;
    border: 1px solid #000;
    border-radius: 0.5rem;
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