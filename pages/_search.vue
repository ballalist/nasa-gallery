<template>
   <section class="section">
      <div class="container">
         <h1 class="title">
            {{ query }} Album.
            <nuxt-link to="/" class="is-size-6"> Back</nuxt-link>
         </h1>

         <vcl-instagram v-if="preload"></vcl-instagram>

         <gallery :images="imageList" :index="index" @close="index = null"></gallery>

         <div v-for="(imageList, listIndex) in chunkedImage" :key="listIndex" class="columns">
            <div v-for="(image, imageIndex) in imageList" :key="imageIndex" class="column image" @click="index = imageIndex" :style="{ backgroundImage: 'url(' + image + ')', width: '300px', height: '200px' }"></div>
         </div>
      </div>
   </section>
</template>

<script>
import axios from 'Axios'
import VueGallery from 'vue-gallery';
import chunk from 'chunk';
import { VclInstagram } from 'vue-content-loading';

export default {
   components: {
      VclInstagram,
      'gallery': VueGallery
   },
   data() {
      return {
         query: this.$route.params.search,
         imageList: [],
         imageThumb: [],
         index: null,
         preload: true,
      }
   },
   methods: {
      init() {
         let self = this
         axios.get('https://images-api.nasa.gov/search?q='+this.query+'&media_type=image').then( (response) => {
            let tempAjax
            tempAjax = response.data.collection.items
            tempAjax.forEach(function(item) {
               let nasaId = item.data[0].nasa_id
               let imageBucket = item.href
               let title = item.data[0].title
               let desc  = ''
               axios.get(imageBucket).then( (cb) => {
                  if(typeof cb.data[0] != 'undefined') {
                     let object = {
                        title: title,
                        description: desc,
                        href: cb.data[0]
                     }
                     self.imageList.push(object)
                  }
               })
               self.imageThumb.push(item.links[0].href)
            });
            self.preload = false
         }).catch( (err) => {
            console.log(err)
         })
      }
   },
   created() {
      this.init()
   },
   computed: {
      chunkedImage() {
         return chunk(this.imageThumb, 4)
      }
   }
}
</script>