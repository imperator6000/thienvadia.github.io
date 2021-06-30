<template>
  <div class="container">
    <control></control>
    <galleries :images="getImages()" :per-page="perPage"></galleries>
  </div>
</template>

<script>
import xdata from './data/images.json'
import Galleries from '~/components/common/galleries'
import Control from '~/components/common/control'


export default {
  components: {
    Galleries,
    Control
  },
  data () {
    return {
      allImages: [],
      perPage: 25,
    }
  },
  methods: {
    getImages () {
      return xdata.map(img => img.url)
    }
  }
}
</script>

<style lang="sass">

</style>
