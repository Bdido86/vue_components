<template>
  <form class="card card-w30" @submit.prevent="submitForm">
    <div class="form-control">
      <label for="type">Тип блока</label>
      <select id="type" v-model="blockType">
        <option v-for="blockType in blockTypes" :key="blockType.key" :value="blockType.key">{{ blockType.value }}</option>
      </select>
    </div>
    <div class="form-control">
      <label for="value">Значение</label>
      <textarea id="value" rows="3" v-model.trim="blockValue"></textarea>
    </div>
    <button class="btn primary" :disabled="canBlockAdd">Добавить</button>
  </form>
</template>

<script>

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
    submitForm() {
      const block = {
        component: this.blockType,
        value: this.blockValue
      }

      this.resetForm()
      this.$emit('createBlockView', block)
    }
  },
  emits: ['createBlockView']
}
</script>
