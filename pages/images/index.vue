<template>
  <div class="container">
    <control></control>
    <galleries ref="XGalleries" :images="allImages" :per-page="perPage"></galleries>
  </div>
</template>

<script>
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
  created() {
    this.getImages()
  },
  methods: {
    async getImages() {
      await fetch(`/images.json`)
        .then(async response => {
          const data = await response.json();
          if (!response.ok) {
            const error = (data && data.message) || response.statusText;
            return Promise.reject(error);
          }
          this.allImages = data
          this.$refs.XGalleries.reset(this.allImages)
        })
    }
  }
}
</script>

<style lang="sass">

</style>
