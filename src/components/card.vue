<template>
     <div class="column is-6-mobile is-3-desktop is-4-tablet">
          <div class="card">
            <div class="card-image">
                <figure class="image is-3by4">
                     <img v-if="card.imageUrl" v-bind:src="card.imageUrl"  v-bind:alt="card.name">
                     <img  v-else src="../assets/noimage.gif">  
                </figure>
             
          </div>
          <div class="card-content">
            <h3 class="title is-size-5">{{card.name}}<br></h3>
            <h4 class="title is-size-6">{{card.types[0]}}</h4>
            <button class="button is-rounded is-small" @click="showModal(card.id)">Ver mas</button>

          </div>
        </div>
       </div>
</template>

<script>
export default {
    props:['card'],


    methods:{
        showModal(id){
            this.$emit('showModal', id)
        },

    }
}
</script>