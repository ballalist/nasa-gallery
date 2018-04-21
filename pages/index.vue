<template>
   <section class="section">
       <div class="container">
         <h1 class="title">
            What you want to see from NASA?
         </h1>
         <div class="columns">
            <div v-for="(c ,index) in cardList" :key="index" class="column">
               <card :search="c" />
            </div>
         </div>
         <div class="columns">
            <div v-for="(c ,index) in cardListRow2" :key="index" class="column">
               <card :search="c" />
            </div>
         </div>
       </div>
     </section>
</template>

<script>
import axios from 'Axios'
import card from '~/components/card.vue'

export default {
  components: {
      card
  },
  data() {
      return {
         cardList: ['The Sun', 'The Moon', 'The Earth', 'The Mars'],
         cardListRow2: ['Mercury Program', 'Apollo Program', 'Skylab', 'Space Shuttle']
      }
  },
  methods: {
   init : function() {
      let self = this

      // Object.keys(this.cardList).map((key, index) => {
      //    let search = this.cardList[key].search
      //    axios.get('https://images-api.nasa.gov/search?q='+search+'&media_type=image').then( (response) => {
      //       console.log(response)
      //       let thumbnailImage             = response.data.collection.items[0].links[0].href
      //       let desc                       = response.data.collection.items[0].data[0].title
      //       self.cardList[key].thumbnail   = thumbnailImage
      //       self.cardList[key].description = desc
      //    }).catch( (err) => {
      //       console.log(err)
      //    })
      // })
   }
  },
  mounted: function() {
   this.init()
  }
}
</script>

