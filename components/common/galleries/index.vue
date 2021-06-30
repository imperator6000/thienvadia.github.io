<template>
  <div class="row">
    <div class="col-12">
      <div id="mygallery" >
        <a v-for="image in goalImages" :key="image" :href="image">
          <img :src="image"/>
        </a>
      </div>
      <span v-if="msg !== ''" class="alert alert-info">{{msg}}</span>
      <infinite-loading ref="infiniteLoading" v-if="!isFirst" @infinite="infiniteHandler"></infinite-loading>
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
      ximages: [],
      allImages: [],
      goalImages: [],
      isLoading: true,
      isFirst: true,
      msg: ''
    }
  },
  mounted() {
    this.init(this.images)
  },
  methods: {
    reset (imgs) {
      this.init(imgs)
    },
    init(imgs) {
      this.ximages = imgs;
      this.allImages = this.shuffle(imgs)
      this.nextPage(() => {
        this.isFirst = false
      })
    },
    infiniteHandler ($state) {
      if (this.isFirst) {
        return
      }
      if (this.goalImages.length >= this.ximages.length) {
        $state.complete()
        return
      }
      console.log(`${this.goalImages.length} / ${this.ximages.length}`)
      this.nextPage(() => {
        if (this.allImages.length === 0) {
          $state.complete()
        } else {
          $state.loaded();
        }
      })
    },
    nextPage (callback = null) {
      if (this.goalImages.length >= this.ximages.length) {
        return
      }
      let ids = new Set(this.goalImages)
      this.allImages = this.allImages.filter(imgSrc => !ids.has(imgSrc))
      this.goalImages = [...this.goalImages, ...this.allImages.slice(0, this.perPage)];
      this.msg = `${this.goalImages.length} / ${this.ximages.length}`;
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
