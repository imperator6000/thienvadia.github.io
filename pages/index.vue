<template>
  <b-container>
    <b-row>
      <b-col class="col-12">
        <router-link to="/images">All Images</router-link>
        <nuxt-link class="link-in-newline" v-for="link in links" :key="link" :to="correctLink(link)">{{ link }}</nuxt-link>
      </b-col>
    </b-row>
  </b-container>
</template>

<script>
export default {
  data () {
    return {
      links: []
    }
  },
  created() {
    this.importAll(require.context('./bai-viet/data/', true, /\.json$/))
  },
  methods: {
    importAll(r) {
      r.keys().forEach(key => {
        this.links.push(key.substring(2, key.lastIndexOf('.json')))
      });
    },
    correctLink (path) {
      return `/bai-viet/${path}`;
    },
  }
}
</script>

<style lang="sass">
.link-in-newline
  background-color: white
  display: block
  padding: 5px 10px
  text-underline: none

.link-in-newline:hover
  background-color: antiquewhite
</style>
