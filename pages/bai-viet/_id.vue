<template>
  <div class="container">
    <div class="row">
      <div class="col-12">
        <router-link to="/">Back</router-link>
      </div>
    </div>
    <div v-for="article in articles" :key="article.uuid" v-html="article.content_html"></div>
  </div>
</template>

<script>
export default {
  data () {
    return {
      fileName: '',
      articles: [],
    }
  },
  created() {
    this.fileName = this.$route.path;
    this.fileName = this.fileName.substring(this.fileName.lastIndexOf('/') + 1, this.fileName.length)
    this.articles = require(`./data/${this.fileName}.json`)
  }
}
</script>

<style lang="sass" scoped>
img
  width: 100%
</style>
