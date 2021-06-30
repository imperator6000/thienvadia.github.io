<template>
  <div class="container">
    <control></control>

    <a v-if="!isShowImages" class="btn btn-primary" href="javascript:;" @click="showImages">Show Images Only</a>
    <a v-else class="btn btn-primary" href="javascript:;" @click="showImages">Show Content</a>

    <div v-if="!isShowImages" v-for="article in articles" :key="article.uuid" v-html="article.content_html"></div>
    <galleries v-else :images="allImages"></galleries>
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
  },
  mounted() {
    this.articles = require(`./data/${this.fileName}.json`)
    let setImgs = new Set()
    this.articles.forEach(item => {
      try {
        let imgs = $(item.content_html).find('img')
        imgs.each((idx, elem) => {
          let m;
          if ((m = this.regex.exec(elem.src)) !== null) {
            setImgs.add(m[0])
          }
        })
      } catch (ignore) {}
      finally {
        this.allImages = Array.from(setImgs)
      }
    })
  },
  methods: {
    showImages () {
      this.isShowImages = !this.isShowImages
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


