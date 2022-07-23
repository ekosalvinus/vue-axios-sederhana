<template>
  <div>
    <form @submit.prevent="save">
      <input type="text" v-model="form.title" placeholder="Judul"> <br><br>
      <textarea v-model="form.content" placeholder="Isi konten"></textarea> <br><br>
      <button type="submit">Save Konten</button>
    </form>
    <h2>Get Data from RESTful API</h2>
    <ul>
      <li v-for="article in articles" :key="article.id">
        {{ article.title }} <br>
        {{ article.content }}
        
      </li>
    </ul>
    <button @click="load">Load Data</button>
  </div>
</template>

<script>
import axios from 'axios'

export default {
  name: 'App',
  data() {
    return {
      form: {
        title:'',
        content:'',
      },
      articles: []
    }
  },

  async mounted() {
    this.load()
  },

  methods: {
    async load() {
      const response = await axios.get(`http://localhost:3000/articles`)
      this.articles = response.data
    },

    async save() {
      try {

      const response = await axios.post(`http://localhost:3000/articles`, this.form)
      this.articles = response.data 
      this.load()
      this.title = ''
      this.content = ''
      
      } catch (e) {
        console.log(e)
      }
    }

  }

}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>