<template>
  <b-container>
    <b-row>
      <b-col class="col-12">
        <router-link class="btn btn-primary mt-1 mr-1" to="/images">All Images</router-link>
        <router-link class="btn btn-success mt-1 mr-1"
                   v-for="fileName in fileNames"
                   :key="fileName"
                   :to="{path: '/bai-viet', query: { fileName }}">{{ fileName }}</router-link>
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
    this.importAll(require.context('./bai-viet/data/', true, /\.json$/))
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
