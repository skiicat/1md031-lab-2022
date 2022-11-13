<template>
    <div>
      <div>
        <h3 class="three-burgers">{{ burger.name }}</h3>
        <img v-bind:src="burger.URL" style="height: 300px"> <!--alt="Bejeweled Burger" title="burger.name"-->
          <ul>
            <li>{{burger.type}}</li>
            <li id="gluten" v-if="burger.gluten">Contains gluten</li>
            <li id="lactose" v-if="burger.lactose">Contains lactose</li>
            <li id="vegan" v-if="burger.vegan">Vegan</li>
          </ul>
        </div> 
    
    </div>
  </template>
  
  <script>
  export default {
    name: 'OneBurger',
    props: {
      burger: Object
    }
  }
  </script>
  
  <!-- Add "scoped" attribute to limit CSS to this component only -->
  <style scoped>
  
  div {
    padding: 5px;
    margin: 10px;
 }

  .three-burgers {
    margin: 0px 0px 10px 0px;
 }

  .box-a {
   grid-column: 1;
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
   font-weight: bold;
}
  </style>
  