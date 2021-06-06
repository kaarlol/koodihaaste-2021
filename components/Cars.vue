<template>
  <div class="cars">
    <!-- Loop through cars array in data to set content -->
    <div v-for="car in cars" :key="car.id" class="car" :class="selectedCar(car.id)" @click="carClick([car.id,car.img,car.consumption])">
      <img :src="car.img" :alt="'Auto ' + car.id">
      <h2>Auto {{ car.id }}</h2>
      <p>Kulutus {{ car.consumption }}l / 100km <br> 1km/h nopeudella</p>
    </div>
  </div>
</template>

<script>
export default {
  props:{
    selected: { // Get earlier selected car id from parent
      type:String,
      default:''
    }
  }, 
  data: function() {
    return {
      cars: [ // Define content for the car selection
        {
          id: 'A',
          img: require('~/assets/auto-a.svg'),
          consumption: 3 
        },
        {
          id: 'B',
          img: require('~/assets/auto-b.svg'),
          consumption: 3.5
        },
        {
          id: 'C',
          img: require('~/assets/auto-c.svg'),
          consumption: 4
        }

      ]
    }
  },
  methods: {
    // Emit car selection to parent (index.vue)
    carClick: function(event) {
      this.$emit('carClick', event)
    },
    // Add selected css class to car if it has been selected earlier
    selectedCar: function(value) {
      return this.selected === value ?  'selected' : ''
    }
  }
}
</script>

<style scoped>
.cars {
    display: flex;
    align-items:center;
    justify-content: center;
    flex-wrap: wrap;
}

.car {
    margin: 2rem;
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

.car img {
    width: 12rem;
}

.car h2 {
    margin: 1rem;
}

.car p {
    margin: 0 0 1rem 0;
}


</style>