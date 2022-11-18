<template>
    <div class="burgerDiv">
      
        <h3>{{ burger.name }}</h3>
        <img v-bind:src="burger.URL" style="height: 300px"> <!--alt="Bejeweled Burger" title="burger.name"-->
          <ul>
            <li>{{burger.type}}</li>
            <li id="gluten" v-if="burger.gluten">Contains gluten</li>
            <li id="lactose" v-if="burger.lactose">Contains lactose</li>
            <li id="vegan" v-if="burger.vegan">Vegan</li>

            <button v-on:click="deleteBurger">-</button>
            {{ amountOrdered }}
            <button v-on:click="addBurger">+</button>
          </ul>

    </div>
  </template>
  
  <script>
  export default {
    name: 'OneBurger',
    props: {
      burger: Object
    },

    data: function () {
  return {
    amountOrdered: 0,

  }
},
    methods: {
      addBurger:function() {
        this.amountOrdered++;
        this.$emit("orderedBurger", { name:   this.burger.name, 
                                      amount: this.amountOrdered 
                              });
      },
      deleteBurger:function() {
        if (this.amountOrdered > 0) {
        this.amountOrdered--;
        this.$emit("orderedBurger", { name:   this.burger.name, 
                                      amount: this.amountOrdered 
                              });}
      },
    }
  }
  </script>
  
  <!-- Add "scoped" attribute to limit CSS to this component only -->
  <style scoped>

 .burgerDiv {
    padding: 5px;
    margin: 10px;
 }

 h3{
    margin: 0px 10px 5px 0px;
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
  