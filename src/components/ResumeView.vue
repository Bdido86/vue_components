<template>
  <div class="card card-w70">
    <template v-if="blocks.length > 0" >
      <component v-for="block in blocks" :is="getComponent(block)" :="bindComponent(block)"></component>
    </template>

    <h3 v-else>Добавьте первый блок, чтобы увидеть результат</h3>
  </div>
</template>

<script>

import ResumeViewAvatar from "./ResumeViewAvatar";
import ResumeViewSubtitle from "./ResumeViewSubtitle";
import ResumeViewText from "./ResumeViewText";
import ResumeViewTitle from "./ResumeViewTitle";

export default {
  props: {
    blocks: {
      required: true,
      type: Object
    }
  },
  methods: {
    bindComponent(block) {
      let prop = {}
      prop[block.component] = block.value
      return prop
    },
    getComponent(block) {
      return 'resume-view-' + block.component
    },
  },
  components: {ResumeViewAvatar, ResumeViewSubtitle, ResumeViewText, ResumeViewTitle}
}
</script>
