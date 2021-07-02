<template>
  <div class="container">
    <control></control>

    <a v-if="!isShowImages" class="btn btn-primary" href="javascript:;" @click="showImages">Show Images Only</a>
    <a v-else class="btn btn-primary" href="javascript:;" @click="showImages">Show Content</a>

    <div v-if="!isShowImages" v-for="article in articles" :key="article.uuid" v-html="article.content_html"></div>
    <galleries v-if="isShowImages" ref="myXGalleries" v-bind:images="allImages"></galleries>
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
    fetch(`/data/${this.fileName}`)
      .then(async response => {
        const data = await response.json();
        if (!response.ok) {
          const error = (data && data.message) || response.statusText;
          return Promise.reject(error);
        }
        this.articles = data.posts
        this.allImages = data.images
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


