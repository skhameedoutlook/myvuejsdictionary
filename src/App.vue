<template>
  <div id="app" class="container">
    <div class="text-center">
      <h1>Type a word to find its meaning</h1>
      <div class="d-flex justify-content-center">
        <input id="mainword"  v-model="inputText" class="form-control-plaintext" style="" /> 
        <button @click="dfd3" class="btn btn-primary" style="height: 40px; border-radius: 0;">Clear Search</button>
      </div>
      <div v-if="disptop10" class="d-flex justify-content-center" style=" text-align: left; ">
        <div style="width: 85%;">
          <h3 style="padding-left: 20px;">Your word could mean:</h3>
          <ul>
            <li class="theItem d-flex justify-content-center" style="text-align: left;" v-for="item in top10" :key="item">{{item}}</li>
          </ul> 
        </div>    
      </div>
      <div v-else class="d-flex justify-content-center" style="text-align: left; margin-top: 10px;">
        <p>No words found.</p>
      </div>
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
      maxCount: 5
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
      if(this.inputText.length == 0) {
        return;
      }
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
    dfd3() {
      this.disptop10 = false;
      this.inputText = "";
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
  
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
#mainword {
  height: 40px; width: 250px; border: 1px solid grey; float: left; 
}
#mainword:focus {
  height: 40px; width: 250px; border-radius: 0; border: 2px solid red; float: left; border-top-left-radius: 5px;border-bottom-left-radius: 5px;
}
.theItem {
  display: block;
  width: 100%;
  background: #ecf0f1;
  border-radius: 5px;
  margin: 0;
  margin-top: 10px;
  margin-bottom: 10px;
  font-size: 18px;
  padding: 8px;
  margin-left: -20px;
  text-align: left;
}
</style>
