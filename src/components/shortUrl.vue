<template>
  <div id="main">
    <h1>PicSee短網址產生器</h1>
    <form @submit.prevent>
      <input type="text" placeholder="請輸入網址" v-model="inputLink">
      <button @click="getUrl"> 送出 </button>
    </form>
    <div class="url-list">
      <p>網址列表：</p>
      <li v-for="(item, index) in allLinks" :key="index">
        <p>原網址：{{ sourceLinks[index] }}</p>
        <a :href="item">{{ item }}</a>
      </li>
    </div>
  </div>
</template>

<script>
import axios from 'axios'
import qs from 'qs'

const API_URL = 'https://api.pics.ee/v1/links?'
export default {
  data () {
    return {
      inputLink: '',
      sourceLinks: [],
      allLinks: []
    }
  },
  methods: {
    async getUrl () {
      const querystring = {
        access_token: '20f07f91f3303b2f66ab6f61698d977d69b83d64',
        caller: 'client-simple',
        lang: 'zh-tw'
      }
      await axios.post(`${API_URL + qs.stringify(querystring)}`, {
        url: this.inputLink
      })
        .then(res => {
          this.sourceLinks.push(this.inputLink)
          this.allLinks.push(res.data.data.picseeUrl)
          console.log(res)
        })
        .catch(err => {
          console.log(err)
        })
    }
  }
}
</script>

<style lang="scss">
#main {
  .url-list {
    li {
      list-style: none;
      // line-height: 1.5;
      padding-top: 10px;
    }
  }
}
</style>
