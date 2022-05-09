<template>
  <div class="container">
    <p v-if="comments.length === 0">
      <button class="btn primary" @click="loadComments">Загрузить комментарии</button>
    </p>
    <div class="card" v-else>
      <h2>Комментарии</h2>
      <ul class="list">
        <li class="list-item" v-for="comment in comments" :key="comment">
          <div>
            <p><strong>{{comment.email}}</strong></p>
            <small>{{comment.text}}</small>
          </div>
        </li>
      </ul>
    </div>

    <app-loader v-if="loading"></app-loader>
  </div>
</template>

<script>

import axios from 'axios'
import AppLoader from "./AppLoader";

export default {
  data() {
    return {
      comments: [],
      loading: false
    }
  },
  methods: {
    async loadComments() {
      try {
        this.loading = true
        const {data} = await axios.get('https://jsonplaceholder.typicode.com/comments?_limit=42')
        data.forEach((item) => {
          this.comments.push({
            'email': item.email,
            'text': item.body
          })
        })

        this.loading = false
      } catch (e) {
        this.loading = false
      }
    },
  },
  components: {AppLoader}
}
</script>
