<template>
  <div class="row">
    <div class="col-12">
      <div id="mygallery" >
        <a v-for="image in goalImages" :key="image" :href="image">
          <img :src="image"/>
        </a>
      </div>
      <infinite-loading @infinite="infiniteHandler"></infinite-loading>
    </div>
  </div>
</template>

<script>
import InfiniteLoading from 'vue-infinite-loading';

export default {
  props: {
    images: {
      type: Array,
      default () {
        return []
      }
    },
    perPage: {
      type: Number,
      default () {
        return 25
      }
    }
  },
  components: {
    InfiniteLoading
  },
  data () {
    return {
      allImages: [],
      goalImages: [],
    }
  },
  created() {
    this.allImages = this.shuffle(this.images)
    this.nextPage()
  },
  methods: {
    infiniteHandler ($state) {
      this.nextPage(() => {
        if (this.allImages.length === 0) {
          $state.complete()
        } else {
          $state.loaded();
        }
      })
    },
    nextPage (callback = null) {
      let ids = new Set(this.goalImages)
      this.allImages = this.allImages.filter(imgSrc => !ids.has(imgSrc))
      this.goalImages = [...this.goalImages, ...this.allImages.slice(0, this.perPage)];
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

<style scoped>

</style>
