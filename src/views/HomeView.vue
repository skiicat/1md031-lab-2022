<template>
  <div>
    <div>
        <header class="header">
            <img id="image" src="https://media.istockphoto.com/id/543441106/photo/close-up-of-meat-patties-on-a-barbecue.jpg?s=612x612&w=0&k=20&c=gRInbV--3Xq6YBVxaWeBJAe3USOJS59gZjt7Qz7nk0s=">
            <h1 id="headline">Welcome to Vigilante Burgers</h1>

        </header>
        <main>
            <section id="burgers">
                <h2>Select burger</h2>
                <p>Please pick your meal from our selection of burgers!</p>
                <!--Burgers here-->
                <div class="wrapper">
                    <div class="box-a">
                        <h3 class="three-burgers">The Bejeweled Burger</h3>
                        <img src="https://www.kitchensanctuary.com/wp-content/uploads/2015/03/Roast-Chicken-brioche-square-720.jpg" alt="Bejeweled Burger" title="Bejeweled Burger" style="height: 300px">
                            <ul>
                                <li>Chicken burger</li>
                                <li id="gluten">Contains gluten</li>
                                <li id="lactose">Contains lactose</li>
                            </ul>
                    </div> 
                
                    <div class="box-b">
                        <h3 class="three-burgers">The Karma Burger</h3>
                        <img src="https://www.kitchensanctuary.com/wp-content/uploads/2015/11/Halloumi-Burger-with-Sticky-Chilli-Glaze-square-FS-50.jpg" alt="Karma Burger" title="Karma Burger" style="height: 300px">
                            <ul>
                                <li>Halloumi burger</li>
                                <li id="gluten">Contains gluten</li>
                                <li id="lactose">Contains lactose</li>
                            </ul>
                    </div>
                    <div class="box-c">
                        <h3 class="three-burgers">The Midnight Burger</h3>
                        <img src="https://cdn.shopify.com/s/files/1/0563/7373/9669/articles/black-burger-bun-featured.jpg?v=1628758353" alt="Midnight Burger" title="Midnight Burger" style="height: 300px">
                            <ul>
                                <li>Bean burger</li>
                                <li>Free of gluten</li>
                                <li id="vegan">Vegan</li>
                            </ul>
                    </div>
                </div>
            </section>
            <section id="order-info">
                <h2>Customer information</h2>
                <p>Please enter essential information here for the delivery</p>
                <h3>Delivery information</h3>
                <p> <!--name-->
                    <label for="name">Full name</label><br>
                    <input type="text" id="name" name="fn" required="required" placeholder="First and last name">
                </p>

                <p><!--e-mail-->
                    <label for="mail">E-mail</label><br>
                    <input type="email" id="mail" name="fn" required="required" placeholder="E-mail address">
                </p>

                <p><!--street-->
                    <label for="street">Street</label><br>
                    <input type="text" id="street" name="fn" required="required" placeholder="Street name">
                </p>

                <p><!--house number-->
                    <label for="house">House</label><br>
                    <input type="number" id="house" name="fn" required="required" placeholder="House number">
                </p>

                <h4>Payment options</h4>
                <p>
                    <label for="recipient"></label><br>
                    <select id="recipient" name="rcp">
                        <option>Credit card</option>
                        <option>Paypal</option>
                        <option>Klarna</option>
                        <option>Swish</option>
                    </select>
                 </p>

                <h4>Gender </h4>
                <p class="gender">
                    <input type="radio" id="female" name="gender" value="female">
                    <label for="female">Female</label>
                    <br>
                    <input type="radio" id="male" name="gender" value="male">
                    <label for="male">Male</label>
                    <br>
                    <input type="radio" id="non-binary" name="gender" value="non-binary">
                    <label for="non-binary">Non-binary</label>
                    <br>
                    <input type="radio" id="no" name="gender" value="no">
                    <label for="no">Do not wish to say</label>
                    <br>
                </p>

            </section>
            <button type="submit">
                <img src="https://cdn-icons-png.flaticon.com/128/3500/3500833.png" style="width: 15px">
                Place Order
            </button>
        </main>
        <hr>
        
        <footer>
            &copy 2022 Vigilante Burgers Inc.
        </footer>
    </div>

    <div>
      Burgers
      <Burger v-for="burger in burgers"
              v-bind:burger="burger" 
              v-bind:key="burger.name"/>
    </div>
    <div id="map" v-on:click="addOrder">
      click here
    </div>
  </div>
</template>

<script>
import Burger from '../components/OneBurger.vue'
import io from 'socket.io-client'

const socket = io();

/*const MenuItem = function(name:, URL, lactose, gluten, vegan) {
  this.name = name;
  this.imageLink = URL;
  this.lactose = lactose;
  this.gluten = gluten;
  this.vegan = vegan;
}*/

const burgerArray = [{name:"The Bejeweled Burger", type:"Chicken burger", URL:"https://www.kitchensanctuary.com/wp-content/uploads/2015/03/Roast-Chicken-brioche-square-720.jpg", lactose:true, gluten:true, vegan:false,}, 
                    {name:"The Karma Burger", type:"Halloumi burger", URL:"https://www.kitchensanctuary.com/wp-content/uploads/2015/11/Halloumi-Burger-with-Sticky-Chilli-Glaze-square-FS-50.jpg", lactose:true, gluten:true, vegan:false}, 
                    {name:"The Midnight Burger", type:"Bean burger", URL:"https://cdn.shopify.com/s/files/1/0563/7373/9669/articles/black-burger-bun-featured.jpg?v=1628758353", lactose:false, gluten:false, vegan:true}
                    ];

export default {
  name: 'HomeView',
  components: {
    Burger
  },
  data: function () {
    return {
      burgers: burgerArray
    }
  },
  methods: {
    getOrderNumber: function () {
      return Math.floor(Math.random()*100000);
    },
    addOrder: function (event) {
      var offset = {x: event.currentTarget.getBoundingClientRect().left,
                    y: event.currentTarget.getBoundingClientRect().top};
      socket.emit("addOrder", { orderId: this.getOrderNumber(),
                                details: { x: event.clientX - 10 - offset.x,
                                           y: event.clientY - 10 - offset.y },
                                orderItems: ["Beans", "Curry"]
                              }
                 );
    }
  }
}
</script>

<style>
  body {
    font-family:'Gill Sans', 'Gill Sans MT', Calibri, 'Trebuchet MS', sans-serif;
}

#burgers {
   margin: 20px 10px;
}

#order-info{
    background-color: black;
    color:aliceblue;
    margin: 20px 10px;
}

button:hover {
    background-color:lightgreen;
    cursor:pointer;
 }

 button {
    margin: 10px 20px;
 }

 section {
    margin: 20px 10px; /*top/bottom right/left*/
    border: dotted; /*automatically becomes color of text*/
 }

 div {
    padding: 5px;
    margin: 10px;
 }

 header {
    margin: 10px;
 }

 h2{
    margin: 10px 20px;
 }

 h3{
    margin: 10px 20px;
 }

 h4{
    margin: 10px 20px;
 }

 p {
    margin: 0px 20px;
 }

 .three-burgers {
    /*margin: 0px 0px 10px 0px;*/
    text-align: center;
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

 .box-a {
   grid-column: 1;
   grid-row: 1;
}
.box-b {
   grid-column: 2 ;
   grid-row: 1;
}
.box-c {
   grid-column: 3;
   grid-row: 1;
}

#gluten {
   color: maroon;
   font-weight:bold;
}

#lactose {
   color:darkblue;
   font-weight: bold;
}

#vegan {
   color:forestgreen;
}

  #map {
    width: 300px;
    height: 300px;
    background-color: red;
  }
</style>