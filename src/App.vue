<template>
  <div class="container">
    <h1>知识问答</h1>

    <input type="file" @change="upload" />
    <p>{{ uploadMessage }}</p>

    <textarea v-model="query" placeholder="请输入问题..." rows="4"></textarea>
    <button @click="ask">提问</button>

    <div v-if="answer">
      <h3>回答：</h3>
      <p>{{ answer }}</p>
    </div>
  </div>
</template>

<script setup>
import { ref } from "vue"
import axios from "axios"

const query = ref("")
const answer = ref("")
const uploadMessage = ref("")

const upload = async (e) => {
  const formData = new FormData()
  formData.append("file", e.target.files[0])
  const res = await axios.post("http://localhost:8000/upload", formData)
  uploadMessage.value = res.data.message
}

const ask = async () => {
  const res = await axios.post("http://localhost:8000/qa", { query: query.value })
  answer.value = res.data.answer
}
</script>

<style>
.container {
  max-width: 700px;
  margin: 30px auto;
  font-family: sans-serif;
}
textarea {
  width: 100%;
  padding: 8px;
  margin-top: 12px;
}
button {
  margin-top: 10px;
  padding: 8px 16px;
}
</style>
