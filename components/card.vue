<template>
   <!-- begin card -->
   <nuxt-link :to="{ name: 'search', params: { search: search }}">
      <vcl-instagram v-if="preload"></vcl-instagram>
      <div class="card" v-if="!preload">
        <div class="card-image">
          <figure class="image is-4by3">
            <img :src="thumbnail" :alt="title">
          </figure>
        </div>
        <div class="card-content">
          <div class="media">
            <div class="media-content">
              <p class="title is-size-4">{{ title }}</p>
              <p class="subtitle">{{ description }}</p>
            </div>
          </div>
        </div>
      </div>
   </nuxt-link>
   <!-- end card -->
</template>

<script>
import axios from 'Axios'
import { VclInstagram } from 'vue-content-loading';

export default {
   props: ['search'],
   components: {
      VclInstagram,
   },
   data() {
      return {
         title: this.search,
         description: '',
         thumbnail: '',
         preload: true
      }
   },
   methods: {
      init() {
         let self = this
         axios.get('https://images-api.nasa.gov/search?q='+this.title+'&media_type=image').then( (response) => {
            let thumbnailImage = response.data.collection.items[0].links[0].href
            let desc           = response.data.collection.items[0].data[0].title
            self.thumbnail     = thumbnailImage
            self.preload = false
         }).catch( (err) => {
            console.log(err)
         })
      }
   },
   created() {
      this.init()
   }
}
</script>