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
       <AppLoader/>
    </div>
  </div>

  <app-comments></app-comments>
</template>

<script>
import axios from 'axios'

import AppTitle from "./components/AppTitle"
import AppSubtitle from "./components/AppSubtitle";
import AppAvatar from "./components/AppAvatar";
import AppText from "./components/AppText";
import AppComments from "./components/AppComments";

const dataBaseUrl = 'https://vue-with-https-9180e-default-rtdb.europe-west1.firebasedatabase.app/block.json'

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
  mounted() {
    this.loadBlocks()
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
    async loadBlocks() {
      const {data} = await axios.get(dataBaseUrl)

      this.blocks = Object.keys(data).map(key => {
        return {
          component: data[key].component,
          value: data[key].value
        }
      })
    },
    async submitForm() {
      const block = {
        component: this.blockType,
        value: this.blockValue
      }

      await axios.post(dataBaseUrl, block)
      this.blocks.push(block)

      this.resetForm()
    }
  },
  components: {AppTitle, AppSubtitle, AppAvatar, AppText, AppComments}
}
</script>
