<template>
  <b-container>
    <b-row>
      <b-col class="col-12">
        <nuxt-link class="btn btn-primary mt-1 mr-1" to="/images">All Images</nuxt-link>
        <nuxt-link class="btn btn-success mt-1 mr-1"
                   v-for="item in fileNames"
                   :key="item.file"
                   :to="{path: '/bai-viet', query: { fileName: item.file }}">{{ item.text }} ( {{ item.total_imgs }} )</nuxt-link>
      </b-col>
    </b-row>
  </b-container>
</template>

<script>
export default {
  data () {
    return {
      fileNames: []
    }
  },
  created() {
    fetch("/authors.json")
      .then(async response => {
        const data = await response.json();
        if (!response.ok) {
          const error = (data && data.message) || response.statusText;
          return Promise.reject(error);
        }
        this.fileNames = data.sort((a, b) => {
          if ( a.total_imgs > b.total_imgs ){
            return -1;
          }
          if ( a.total_imgs < b.total_imgs ){
            return 1;
          }
          return 0;
        })
      })
  },
  methods: {
    importAll(r) {
      r.keys().forEach(key => {
        this.fileNames.push(key.substring(2, key.lastIndexOf('.json')))
      });
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
