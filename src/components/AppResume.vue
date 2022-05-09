<template>
  <div class="container column">
    <resume-form @createBlockView="createBlock"></resume-form>
    <resume-view :blocks="blocks"></resume-view>
  </div>
</template>

<script>

const dataBaseUrl = 'https://vue-with-https-9180e-default-rtdb.europe-west1.firebasedatabase.app/block.json'

import axios from "axios";

import ResumeForm from "./ResumeForm";
import ResumeView from "./ResumeView";

export default {
  data() {
    return {
      blocks: []
    }
  },
  mounted() {
    this.loadBlocks()
  },
  computed: {
    canBlockAdd() {
      return this.blockValue.length < 4
    }
  },
  methods: {
    async loadBlocks() {
      const {data} = await axios.get(dataBaseUrl)
      if (data) {
        this.blocks = Object.keys(data).map(key => {
          return {
            component: data[key].component,
            value: data[key].value
          }
        })
      }
    },
    async createBlock(block) {
      await axios.post(dataBaseUrl, block)
      this.blocks.push(block)
    }
  },
  components: {ResumeForm, ResumeView}
}
</script>

<style scoped>

</style>
