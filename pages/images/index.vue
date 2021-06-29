<template>
  <div class="container">
    <div class="row">
      <div class="col-12">
        <div id="mygallery" >
          <a v-for="image in images" :key="image.url" :href="image.url">
            <img :src="image.url"/>
          </a>
        </div>
        <a class="btn btn-success" href="javascript:;" @click="nextPage">{{ loadMoreText }}</a>
      </div>
    </div>
  </div>
</template>

<script>
import xdata from './data/images.json'

export default {
  data () {
    return {
      allimages: [],
      images: [],
      offset: 0,
      perPage: 25,
      loadMoreText: 'Load more'
    }
  },
  created() {
    this.allimages = this.shuffle(xdata)
    this.nextPage()
  },
  methods: {
    nextPage () {
      let ids = new Set(this.images.map(d => d.url))
      this.allimages = this.allimages.filter(d => !ids.has(d.url))
      this.images = [...this.images, ...this.allimages.slice(0, 25)];
      this.loadMoreText = `Load more (${this.images.length}/${xdata.length})`
      this.$nextTick(() => {
        $('#mygallery').justifiedGallery({
          rowHeight : 300,
          lastRow : 'nojustify',
          margins : 3
        });
      })
    },
    getImages (from, to) {
      return this.shuffle(xdata).slice(from, to)
    },
    shuffle(array) {
      let currentIndex = array.length, temporaryValue, randomIndex;
      while (0 !== currentIndex) {
        randomIndex = Math.floor(Math.random() * currentIndex);
        currentIndex -= 1;
        temporaryValue = array[currentIndex];
        array[currentIndex] = array[randomIndex];
        array[randomIndex] = temporaryValue;
      }
      return array;
    },
  }
}
</script>

<style lang="sass">

</style>
