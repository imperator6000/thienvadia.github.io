<template>
  <div class="container">
    <div class="row">
      <div class="col-12">
        <div id="mygallery" infinite-scroll-disabled="autoLoadDisabled" >
          <a v-for="image in images" :key="image.url" :href="image.url">
            <img :src="image.url"/>
          </a>
        </div>
        <infinite-loading @infinite="infiniteHandler"></infinite-loading>
        <a class="btn btn-success" href="javascript:;" @click="nextPage">{{ loadMoreText }}</a>
      </div>
    </div>
  </div>
</template>

<script>
import xdata from './data/images.json'
import InfiniteLoading from 'vue-infinite-loading';


export default {
  components: {
    InfiniteLoading
  },
  data () {
    return {
      allimages: [],
      images: [],
      offset: 0,
      perPage: 25,
      loadMoreText: 'Load more',
      loading: false,
    }
  },
  created() {
    this.allimages = this.shuffle(xdata)
    this.nextPage()
  },
  computed: {
    autoLoadDisabled() {
      return this.loading || this.allimages.length === 0;
    },
  },
  methods: {
    infiniteHandler ($state) {
      this.nextPage(() => {
        $state.loaded();
      })
    },
    nextPage (callback = null) {
      let ids = new Set(this.images.map(d => d.url))
      this.allimages = this.allimages.filter(d => !ids.has(d.url))
      this.images = [...this.images, ...this.allimages.slice(0, 25)];
      this.loadMoreText = `Load more (${this.images.length}/${xdata.length})`
      this.$nextTick(() => {
        $('#mygallery').justifiedGallery({
          rowHeight : 300,
          lastRow : 'nojustify',
          margins : 3
        }).on('jg.complete', function () {
          if (callback != null) {
            callback()
          }
          $(this).find('a').colorbox({
            maxWidth : '80%',
            maxHeight : '80%',
            opacity : 0.8,
            transition : 'elastic',
            current : ''
          });
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
