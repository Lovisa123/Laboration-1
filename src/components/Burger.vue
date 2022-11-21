<template>
  <div>    
                  <h3> {{ burger.name }} </h3>  
                <img v-bind:src="burger.URL" width="160" height="115"> 
                <ul>
                  <li class="gluten" v-if="burger.gluten"> Contains gluten </li>
                  <li class="vegetarian" v-if="burger.vegetarian"> vegetarian </li>

                  <button v-on:click = "addBurger">+</button>
                  {{ amountOrdered }} 
                  <button v-on:click = "deleteBurger">-</button>
                
              
                </ul> 
              </div>

</template>

<script>

export default {
  name: 'Burger',
  props: {
    burger: Object
  },
  data: function () {
  return {
    amountOrdered: 0,
    
        }
  
},

methods:{
  addBurger: function () {
  this.amountOrdered++;
  this.$emit('orderedBurger', { name:   this.burger.name, 
                                amount: this.amountOrdered 
                              });
    },
    deleteBurger: function (){
      this.amountOrdered--;
      this.$emit('orderedBurger', { name:   this.burger.name, 
                                amount: this.amountOrdered 
                              });
    }
  }
}

</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.gluten {
    color: #ff5500;
    font-weight: bold;
 }
 .vegetarian {
    color: #077d13;
    font-weight: bold;
 }

</style>
