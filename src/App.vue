<template>
  <div class="container column">
    <form class="card card-w30" @submit.prevent="submitForm">
      <div class="form-control">
        <label for="type">Тип блока</label>
        <select id="type" v-model="blockType">
          <option v-for="blockType in blockTypes" :key="blockType.key" :value=blockType.key>{{blockType.value}}</option>
        </select>
      </div>
      <div class="form-control">
        <label for="value">Значение</label>
        <textarea id="value" rows="3" v-model.trim="blockValue"></textarea>
      </div>
      <button class="btn primary" :disabled="canBlockAdd">Добавить</button>
    </form>

    <div class="card card-w70">
       <component v-if="blocks.length > 0" v-for="block in blocks" :is="getComponent(block)" :="bindComponent(block)"></component>

       <h3 v-else>Добавьте первый блок, чтобы увидеть результат</h3>
    </div>
  </div>

  <app-comments></app-comments>
</template>

<script>
import AppTitle from "./components/AppTitle"
import AppSubtitle from "./components/AppSubtitle";
import AppAvatar from "./components/AppAvatar";
import AppText from "./components/AppText";
import AppComments from "./components/AppComments";

export default {
  data() {
    return {
      blockTypes: [
        {'key': 'title', 'value': 'Заголовок'},
        {'key': 'subtitle', 'value': 'Подзаголовок'},
        {'key': 'avatar', 'value': 'Аватар'},
        {'key': 'text', 'value': 'Текст'},
      ],
      blockType: 'title',
      blockValue: '',
      blocks: []
    }
  },
  computed: {
    canBlockAdd() {
      return this.blockValue.length < 4
    }
  },
  methods: {
    resetForm() {
      this.blockValue = ''
      this.blockType = 'title'
    },
    bindComponent(block) {
      let prop = {}
      prop[block.component] = block.value
      return prop
    },
    getComponent(block) {
      return 'app-' + block.component
    },
    submitForm() {
      this.blocks.push({
        'component': this.blockType,
        'value': this.blockValue
      })

      this.resetForm()
    }
  },
  components: {AppTitle, AppSubtitle, AppAvatar, AppText, AppComments}
}
</script>
