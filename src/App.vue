<template>
  <div id="app">
    <input type="text" v-model="inputText" /> 
    <button @click="dfd2">Display Fetched Data</button>
    <div v-if="disptop10">
      <ul>
        <li v-for="item in top10" :key="item">{{item}}</li>
      </ul>
    </div>
  </div>
</template>

<script>

const axios = require('axios').default;

export default {
  name: 'App',
  data() {
    return {
      inputText: "",
      post: "",
      top10: [],
      disptop10: false,
      maxCount: 10
    }
  },
  components: {
    
  },
  created() {
    axios
      .get("https://jsonplaceholder.typicode.com/posts")
      .then(res => {
        this.post = res.data;
      })
  },
  methods: {
    dfd() {
      console.log(this.post);
    },
    dfd2() {
      var temp = this.post.length;
      console.log(this.post);
      var count = 0;
      this.top10 = []
      for(var i = 0; i < temp; i++) {
        if(this.post[i].title.includes(this.inputText)) {
          console.log(this.post[i].title);
          this.top10.push(this.post[i].title);
          count++;
        }
        if(count == this.maxCount) {
          this.displayTop10();
          break;
        }
      }
    },
    displayTop10() {
      this.disptop10 = true;
    }
  },
  watch: {
    inputText: function() {
      this.dfd2();
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
