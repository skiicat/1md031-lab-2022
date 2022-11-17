<template>
  <div>
    <header class="header">
      <img id="image" src="https://media.istockphoto.com/id/543441106/photo/close-up-of-meat-patties-on-a-barbecue.jpg?s=612x612&w=0&k=20&c=gRInbV--3Xq6YBVxaWeBJAe3USOJS59gZjt7Qz7nk0s=">
      <h1 id="headline">Welcome to Vigilante Burgers</h1>
    </header>
    <main>

      <section id="burgers">
        <h2>Select burger</h2>
        <p>Please pick your meal from our selection of burgers!</p>
          <div class="wrapper">
              <Burger v-for="burger in burgers"
                      v-bind:burger="burger" 
                      v-on:orderedBurger="addToOrder($event)"
                      v-bind:key="burger.name"/>
          </div>
      </section>

      <section id="order-info">
        <h2>Customer information</h2>
        <p>Please enter essential information here for the delivery</p>
        <h3>Delivery information</h3>
                  <p> 
                      <label for="name">Full name</label><br>
                      <input type="text" v-model="name" required="required" placeholder="First and last name" />
                  </p>

                  <p>
                      <label for="mail">E-mail</label><br>
                      <input type="email" v-model="mail" required="required" placeholder="E-mail address" />
                  </p>
                    
        <h4>Payment options</h4>
            <p>
                <label for="recipient"></label><br>
                <select id="recipient" v-model="rcp">
                    <option >Credit card</option>
                    <option>Paypal</option>
                    <option>Klarna</option>
                    <option>Swish</option>
                </select>
            </p>


          <h4>Gender </h4>
          <p class="gender" >
                <input type="radio" id="female" v-model="gender" value="female">
                <label for="female">Female</label>
                <br>
                <input type="radio" id="male" v-model="gender" value="male">
                <label for="male">Male</label>
                <br>
                <input type="radio" id="nonBinary" v-model="gender" value="non-binary">
                <label for="non-binary">Non-binary</label>
                <br>
                <input type="radio" id="no" v-model="gender" value="undisclosed">
                <label for="undisclosed">Do not wish to say</label>
                <br>
          </p>

        <h4>Please indicate point of delivery</h4>
        <div id="map">
          <div id="dots" v-on:click="setLocation">
            <div v-bind:style="{ left: location.x + 'px', top: location.y + 'px'}" >T</div>
          </div>
        </div>
 
      </section>
    </main>

      <button type="submit" v-on:click="sendOrder(key)">
          <img src="https://cdn-icons-png.flaticon.com/128/3500/3500833.png" style="width: 15px">
            Place Order
      </button>
      <hr>
          
      <footer>
        &copy; 2022 Vigilante Burgers Inc.
      </footer>
        
  </div>
</template>

<script>
import Burger from '../components/OneBurger.vue'
import io from 'socket.io-client'
import menu from '../assets/menu.json'


const socket = io();

/*function MenuItem(name:, URL, lactose, gluten, vegan) {
  this.name = name;
  this.imageLink = URL;
  this.lactose = lactose;
  this.gluten = gluten;
  this.vegan = vegan;
}*/

/*const burgerArray = [{name:"The Bejeweled Burger", type:"Chicken burger", URL:"https://www.kitchensanctuary.com/wp-content/uploads/2015/03/Roast-Chicken-brioche-square-720.jpg", lactose:true, gluten:true, vegan:false}, 
                    {name:"The Karma Burger", type:"Halloumi burger", URL:"https://www.kitchensanctuary.com/wp-content/uploads/2015/11/Halloumi-Burger-with-Sticky-Chilli-Glaze-square-FS-50.jpg", lactose:true, gluten:true, vegan:false}, 
                    {name:"The Midnight Burger", type:"Bean burger", URL:"https://cdn.shopify.com/s/files/1/0563/7373/9669/articles/black-burger-bun-featured.jpg?v=1628758353", lactose:false, gluten:false, vegan:true}
                    ];*/
  

export default {
  name: 'HomeView',
  components: {
    Burger
  },
  data: function () {
    return {
      selectedBurger: [],
      burgers: menu,
      name:"",
      mail:"",
      gender:"female",
      rcp:"recipient",
      orderedBurgers: {},
      location: { x: 0,
                  y: 0
                }

    }
  },
  methods: {
    setLocation: function(event) {
      var offset = {x: event.currentTarget.getBoundingClientRect().left,
                    y: event.currentTarget.getBoundingClientRect().top};
      this.location = { x: event.clientX - 10 - offset.x,
                        y: event.clientY - 10 - offset.y }
      
    },
    sendOrder: function() {
      //console.log(this.name, this.mail, this.rcp, this.gender, this.orderedBurgers)

      socket.emit("addOrder", { orderId: this.getOrderNumber(),
                                details: this.location,
                                orderItems: this.orderedBurgers,
                                info: {name: this.name, mail: this.mail, gender: this.gender, payment: this.rcp}
                              }
                 );
  },
    addToOrder:function(event) {
      this.orderedBurgers[event.name] = event.amount;
      
    },
    getOrderNumber: function () {
      return Math.floor(Math.random()*100000);
    },

    addOrder: function () {
      /*var offset = {x: event.currentTarget.getBoundingClientRect().left,
                    y: event.currentTarget.getBoundingClientRect().top};
      this.location = { x: event.clientX - 10 - offset.x,
                        y: event.clientY - 10 - offset.y }
                
      socket.emit("addOrder", { orderId: this.getOrderNumber(),
                                details: { x: event.clientX - 10 - offset.x,
                                           y: event.clientY - 10 - offset.y },
                                orderItems: ["Beans", "Curry"]
                              }
                 );*/
    }
  }
  
  }
</script>

<style>
  body {
    font-family:'Gill Sans', sans-serif;
}

#burgers {
   margin: 20px 10px;
}

#order-info{
    background-color: black;
    color:aliceblue;
    margin: 20px 10px;
    overflow:scroll;
}

button:hover {
    background-color:lightgreen;
    cursor:pointer;
 }

 button {
    margin: 10px 20px;
 }

 section {
    margin: 20px 10px;
    border: dotted; 
 }

 header {
    margin: 10px;
 }

 h2 {
    margin: 10px 20px;
 }

 h3 {
    margin: 10px 20px;
 }

 h4 {
    margin: 10px 20px;
 }

 p {
    margin: 0px 20px;
 }

 .gender {
    margin: 0px 0px 10px 10px; /*top right bottom left*/ 
 }
 
 .header {
   margin: 20px 10px;
   height: 150px;
   overflow: hidden;
 }

 #headline {
   position: absolute;
   margin-top: -525px;
   margin-left: 30px;
 }

 #image {
   opacity: 0.6;
   width: 100%;
   height: auto;
 }

 .wrapper {
   display: grid;
   grid-gap: 100px;
   grid-template-columns: 325px 325px 325px;
   background-color: #fff;
   color: #444;
 }

#map {
    width: 1400px;
    height: 500px;
    overflow:scroll;
    
    color: black;
}

#dots {
    position: relative;
    margin: 0;
    padding: 0;
    background-repeat: no-repeat;
    background: url("../../public/img/polacks.jpg");
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