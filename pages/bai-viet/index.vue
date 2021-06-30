<template>
  <div class="container">
    <control></control>

    <a v-if="!isShowImages" class="btn btn-primary" href="javascript:;" @click="showImages">Show Images Only</a>
    <a v-else class="btn btn-primary" href="javascript:;" @click="showImages">Show Content</a>

    <div v-if="!isShowImages" v-for="article in articles" :key="article.uuid" v-html="article.content_html"></div>
    <galleries v-else ref="myXGalleries" v-bind:images="allImages"></galleries>
  </div>
</template>

<script>
import Control from '~/components/common/control'
import Galleries from '~/components/common/galleries'

export default {
  components: {
    Control,
    Galleries
  },
  data () {
    return {
      fileName: '',
      articles: [],
      isShowImages: false,
      allImages: [],
      regex: /https:\/\/(s1\.uphinh\.org|greenupload\.com|upload69\.com)([a-zA-Z0-9\/\-_.\(\) ]+)\.([a-zA-Z]{2}\.[a-zA-Z]{3,4}|[a-zA-Z]{3,4})/gm
    }
  },
  created() {
    this.fileName = this.$route.query.fileName || ''
    this.articles = require(`./data/${this.fileName}.json`)
  },
  methods: {
    getAllImages(articles) {
      return new Promise(resolve => {
        let setImgs = new Set()
        articles.forEach(item => {
          try {
            let m;
            while ((m = this.regex.exec(item.content_html)) !== null) {
              setImgs.add(m[0])
            }
          } catch (ignore) {}
        })
        resolve(Array.from(setImgs))
      })
    },
    showImages () {
      this.isShowImages = !this.isShowImages
      if (this.isShowImages) {
        this.getAllImages(this.articles)
          .then(images => {
            this.allImages = images
            this.$refs.myXGalleries.reset(this.allImages)
          })
      } else {
        this.allImages = []
      }
    }
  }
}
</script>

<style lang="sass">
img, .bbImage
  width: 100%

.smilie
  width: inherit
</style>


