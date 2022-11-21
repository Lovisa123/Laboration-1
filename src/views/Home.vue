<template>
  <header class="test" >
        <img id="image" src="https://pokeburger.se/wp-content/uploads/poke_square1-1024x1024.jpg">
        <h1 id="headline"> Welcome to BurgerOnline </h1>
  </header>

  <main>
    
    <section id= "Selectburgers"> 
         <h3> Select burgers </h3>
         <h3>  Here are our burgers: </h3> 
         <br>


         <div class="burger">
          <Burger v-for="burger in burgers"
              v-bind:burger="burger" 
              v-bind:key="burger.name"
              v-on:orderedBurger="addToOrder($event)"
              />
          </div>

        
    </section>
      
    <section id= "CustomerInformation"> 
     <div> 
       <h3>   Customer information </h3>
       <h5> This is where you provide necessary information </h5>
            
       <h3> Delivery information </h3> 
        <p>
              
          <label for="fullname"> Full name </label><br>
          <input type="text" id="fullname" v-model="name" required="required" placeholder="First - and Last name">
        </p>
          <p>
            <label for="E-mail"> E-mail </label><br>
            <input type="email" id="E-mail" v-model="mail" required="required" placeholder="E-mail address">
          </p>
          <!--
            <p>
              <label for="Street"> Street </label><br>
              <input type="text" id="Street" v-model="street" required="required" placeholder="Street name ">
              </p>
            <p>
                <label for="House"> House </label><br>
                <input type="number" id="House" v-model="house" required="required" placeholder="House number ">
            </p>
            -->

              <p>
                <label for="Payment"> Payment </label><br>
                <select id="Payment" v-model="rcp">
                  <option>Swish </option>
                  <option>Klarna</option>
                  <option>Kontonummer</option>
                  <option>Kontant</option>
                </select>
            </p>
            

              <p>
                <label for= "radio">Gender</label><br>
                <input type="radio" id="Female" v-model="gender" value="Female">
                <label for="Female">Female </label><br>

                <input type="radio" id="Male" v-model="gender" value="Male">
                <label for="Male">Male</label><br>

                <input type="radio" id="Non-binary" v-model="gender" value="Non-binary">
                <label for="Non-binary">Non-binary</label>

                </p>


                 <h3> Please indicate point of delivery:</h3>
                <div id="map">
                 click here
                  
                 <div id="dots" v-on:click="setLocation" >
                   
                    <div v-bind:style="{ left: location.x + 'px', top: location.y + 'px'}" > T
                   </div> 
                   
                 </div>
                 
               </div>
                
       </div>
   </section>
  </main>


    
        
        
        <button type="submit" class="btn" v-on:click="sendOrder">
            <img src="https://cdn-icons-png.flaticon.com/512/4379/4379581.png" 
                alt="Testbild" width="70" height="70"> 
            send my order 
        </button>

        <hr> 
  <footer>
        &copy; Copyright
  </footer>
 
  
</template>


<script>
import Burger from '../components/Burger.vue'
import io from 'socket.io-client'
import menu from '../assets/menu.json'

const socket = io();

/*
function MenuItem(name: , URL, gluten, vegetarian){
  this.name = name ;
  this.imagelink = URL;
  this.gluten = gluten; 
  this.vegetarian = vegetarian;
}
const BurgerArray = [ {name: "The Classic Burger", gluten:true , vegetarian:false, URL: "https://cdn.cnn.com/cnnnext/dam/assets/220428140436-04-classic-american-hamburgers-full-169.jpg"}, 
                      {name:"Chicken Burger", gluten:true, vegetarian: false, URL:"https://www.kitchensanctuary.com/wp-content/uploads/2019/08/Crispy-Chicken-Burger-square-FS-4518.jpg"},
                    {name:"Halloumi Burger", gluten:true, vegetarian:true, URL: "https://www.thelastfoodblog.com/wp-content/uploads/2017/04/Halloumi-Burgers-thumb.jpg"}
                    ];
*/


export default {
  name: 'Home',
  components: {
    Burger
  },
  data: function () {
    return {
      burgers: menu,
      gender: "Female",
      rcp: "Payment",
      name:"",
      mail:"",
      /*street:"",
      house:"",
      */
      orderedBurgers:{},
      location: { x: 0,
            y: 0
          }

       

    }
  },
  methods: {

setLocation: function(event) {
  var offset = {x: event.currentTarget.getBoundingClientRect().left,
                    y: event.currentTarget.getBoundingClientRect().top}
      this.location = { x: event.clientX - 10 - offset.x,
                        y: event.clientY - 10 - offset.y }

},

    addToOrder: function (event) {
  this.orderedBurgers[event.name] = event.amount;
  console.log(this.orderedBurgers)
},

    sendOrder: function (){
      socket.emit("addOrder", { orderId: this.getOrderNumber(),
                                details: this.location ,
                               orderItems: this.orderedBurgers, 
                               namec: this.name,
                               gender:this.gender,
                               payment:this.rcp,
                               email:this.mail

                              }
                 );
      console.log(this.name,this.mail, this.rcp, this.gender, this.orderedBurgers)
    },

    getOrderNumber: function () {
      return Math.floor(Math.random()*100000);
    },
    addOrder: function (event) {
      var offset = {x: event.currentTarget.getBoundingClientRect().left,
                    y: event.currentTarget.getBoundingClientRect().top};
      this.location = { x: event.clientX - 10 - offset.x,
                        y: event.clientY - 10 - offset.y }
                        /*
      socket.emit("addOrder", { orderId: this.getOrderNumber(),
                                details: { x: event.clientX - 10 - offset.x,
                                           y: event.clientY - 10 - offset.y },
                                orderItems: ["Beans", "Curry"]
                              }
                 );
                 */
    }
    
  }
}
</script>

<style>
body {
    font-family: 'Times New Roman', Times, serif ;
}
.gluten {
    color: #ff5500;
    font-weight: bold;
 }

 .vegetarian {
    color: #077d13;
    font-weight: bold;
 }
 
 #Selectburgers {
    background-color:black;
    color:white;
    margin-top: 1.25em;
    margin-bottom: 1.25em;
    margin-right: 5em;
    margin-left: 3.125em; 
    border: 0.5em dotted white;
    padding-left: 0.75em;

    
 }

 #CustomerInformation {
    background-color: white;
    color: black;
    margin-top: 2.5em;
    margin-right: 5em;
    margin-left: 3.125em; 
    border: 0.5em dotted black;
    padding-left: 0.75em;
   
    
 }

 .btn {
    margin-top: 1.875em;
    margin-left: 3.125em;
    margin-bottom: 1.25em;
 }

 .btn:hover {
    background-color: greenyellow;
    color: black;
    cursor: pointer;
    margin-top: 1.875em;
    margin-left: 3.125em;
 }

 .test{
    margin-top: 1.25em;
    margin-bottom: 1.25em;
    margin-right: 5em;
    margin-left: 3.125em; 
    height: 11.25em;
    overflow: hidden;
 }
 

 .burger{
    display: grid;
    grid-gap: 7.8125em;
    grid-template-columns: 7.8125em 7.8125em 7.8125em;
}


 #image {
    opacity: 0.5;
    width: 100%;
    height: auto; 

 }

 #headline{
    position:absolute;
    margin-top: -38.5625em;
    padding-left: 3.125em;
    
 }
  #map {
    width: 1000px; 
    height: 700px;
    overflow:scroll;
  }

  #dots {
  position: relative;
  margin: 0;
  padding: 0;
  background:url("/img/polacks.jpg");
  background-repeat: no-repeat;
  width:1920px;
  height: 1078px;
  cursor: crosshair;
}

#dots div {
  position: absolute;
  background: black;
  color: white;
  border-radius: 10px;
  width:20px;
  height:20px;
  text-align: center;
}
</style>

