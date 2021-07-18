<template>
                <!--NAVBAR COMPONENT-->
  <navbar/>
                <!--EDN NAVBAR COMPONENT-->

  <div id="app">    

                <!-- HERO SECTION-->
    <div class="hero is-white is-gradient is-bold">
      <div class="hero-body">
        <h1 class="title">

          <div class="image">
            <img class="is-full center" src="./assets/logo1.png" alt=""> 
            App
          </div>
        </h1>
                <!--BUTTONS TO SEARCH BY TYPE OF CARD-->
        <div >
          <button class="button is-rounded" v-on:click="typeCard('creature')">Creature</button>
          <button class="button is-rounded" v-on:click="typeCard('Instant')">Instant</button>
          <button class="button is-rounded" v-on:click="typeCard('Enchantment')">Enchantment</button>
          <button class="button is-rounded" v-on:click="typeCard('Sorcery')">Sorcery</button>
          <button class="button is-rounded" v-on:click="typeCard('Land')">Land</button>
          <button class="button is-rounded" v-on:click="typeCard('Artifact')">Artifact</button>
        </div>
              <!--END OF THE BUTTONS TO SEARCH BY CARD TYPE-->
      </div>
    </div>
               <!--END SECTION HERO-->     


               <!-- CARDS SECTION-->
    <div class="container">
      <figure v-if="load" class="image imag" >
      <img class="is-rounded" src="./assets/tenor.gif" alt="">
      <p v-if="load">Loading Cards...</p>
      </figure>
        
     <div v-else class="columns is-desktop is-mobile is-tablet is-multiline is-centered p3">
       <card @showModal="showModal" v-for="card of cards " :key="card.id" :card="card"/>
     </div>
    </div>
              <!--END CARDS SECTION-->


              <!--PAGINATION SECTION-->
    <footer class="footer"> 
      <div class="p3"> 
      <div class="pagination is-centered" role="navigation" aria-label="pagination">
      <a class="pagination-previous" v-on:click="callCardApiRest()">Previous</a>
      <a class="pagination-next" v-on:click="callCardApiSum()">Next Page</a>
      <ul class="pagination-list">
        <li>
          <a class="pagination-link is-current">{{page}}</a>
        </li>
        <li>
          <span class="pagination-ellipsis">Of</span>
        </li>
        <li>
          <a class="pagination-link">{{pages}}</a>
        </li>
      </ul>
    </div>

    </div>
    </footer>
            <!--END PAGINATION SECTION-->


            <!--MODAL SECTION (CARDS BY ID)-->
    <div class="modal" :class="{'is-active':modal}" v-if="modal">
      <div class="modal-background" @click="modal = false">
        <div class="modal-card">
           <div class="modal-card-head">
              <h3 class="modal-card-title">
                Card Name: {{cardIndividual.card.name}}
              </h3>
           </div>
          <div class="modal-content">
            <p class="image  is-1by1">
              <img v-if="cardIndividual.card.imageUrl" v-bind:src="cardIndividual.card.imageUrl" alt="">
              <img  v-else src="./assets/noimage.gif">  
            </p>
          </div>
          
          <footer class="modal-card-foot ">
            <strong>Type</strong>
              <p>{{cardIndividual.card.type}}</p><br>
            <strong>Description</strong>
              <div> 
                <p v-if="cardIndividual.card.originalText">{{cardIndividual.card.originalText}}</p><br>
              </div>
            <button  class="button is-centered">
              Cerrar
            </button>
           </footer>
        </div>
      </div>
    </div>
               <!--END MODAL SECTION (CARDS BY ID)-->
    
    
  </div>
</template>

<script>
//librerias
import axios from "axios";//USE THE SDK
import card from './components/card'
const mtg = require('mtgsdk');
import navbar from './components/navbar';

export default {
  name: "App",
  components:{
    card, 
    navbar
  },
  data: function(){
    return {
      cards:[],
      page:0,
      pages:100,
      modal:false, 
      cardIndividual:{},
      load:false,
      search: ''
    }
  },
  created (){
    this.callCardApiSum();
  },
  methods: {
        //CALL THE NEXT PAGE AND THE MAIN PAGE
   async callCardApiSum(){
      this.cards = [];
      this.page++;
      this.load = true;
        mtg.card.where({ page:this.page, pageSize: 10})
      .then(cartas => {
        this.cards = [];
        this.cards = cartas;
        this.load = false;
      console.log(this.cards)
     })
      window.scrollTo(0,135)
    },
          //CALL THE PREVIOUS PAGE
    callCardApiRest(){
      if(this.page >=2){
        this.cards = [];
      this.page--;
      this.load = true;
       mtg.card.where({ page:this.page, pageSize: 10})
      .then(cartas => {
        this.cards = [];
        this.cards = cartas;
        this.load = false;
      console.log(this.cards)
       })
      window.scrollTo(0, 135)
      }
    },

     
    //CALL THE CARD BY THE TYPE (BY ID)
     typeCard(e){
      this.load = true;
      this.cards = [];
      mtg.card.where({ supertypes: '', types: e })
        .then(cardsType => {
          this.cards = cardsType;
          console.log(cardsType, "este es el tipo de tarjeta") // "Squee, Goblin Nabob"
          this.load = false;
       })
      window.scrollTo(0,135)
      },

    next(e){
      pruebaaa(e);
    },

      
      //AS AN EXAMPLE
      fetch(){
      
      let result = axios
      .get("https://api.magicthegathering.io/v1/cards")
      .then(res =>{
        this.cards = res.data.cards;
        console.log(this.cards);
      })
        
    },
          //CALL THE FUNCTION FOR THE MODAL
    async showModal(id){
      this.fetchUnic(id);
    },
        //FUNCTION FOR THE MODAL OF (BY ID)
     fetchUnic(id){
       this.load = true;
       let individual =  mtg.card.find(id)
         .then(result => {
            this.modal = true;
            this.cardIndividual = result;
            console.log(this.cardIndividual.card.name, "cartaIndividual")
            this.load = false;
      });
        
    },

  },

  
};


// navbar responsive
document.addEventListener('DOMContentLoaded', () => {

  // Get all "navbar-burger" elements
  const $navbarBurgers = Array.prototype.slice.call(document.querySelectorAll('.navbar-burger'), 0);

  // Check if there are any navbar burgers
  if ($navbarBurgers.length > 0) {

    // Add a click event on each of them
    $navbarBurgers.forEach( el => {
      el.addEventListener('click', () => {

        // Get the target from the "data-target" attribute
        const target = el.dataset.target;
        const $target = document.getElementById(target);

        // Toggle the "is-active" class on both the "navbar-burger" and the "navbar-menu"
        el.classList.toggle('is-active');
        $target.classList.toggle('is-active');

      });
    });
  }

});
//fin navbar responsive
</script>

<style>
#app {
  font-family:"Magic:the Gathering";
  text-align: center;
}
</style>
