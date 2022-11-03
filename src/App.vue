<template>
  <div class="header">
    <ul class="header-button-left">
      <li @click="step = 0">Cancel</li>
    </ul>
    <ul class="header-button-right">
      <li v-if="step !== 2" @click="if (step == 1) step = 2;">Next</li>
      <li v-if="step == 2" @click="clickUpload">Upload</li>
    </ul>
    <img src="./assets/logo.png" class="logo" />
  </div>

  <Container
    @upload="postStr = $event"
    :imgUrl="imgUrl"
    :step="step"
    :postData="postData"
    :choiceFilter="choiceFilter"
  />
  <button v-if="step == 0" @click="more">더보기</button>

  <div class="footer">
    <ul class="footer-button-plus">
      <input @change="fileUpload" type="file" id="file" class="inputfile" />
      <label for="file" class="input-plus">+</label>
    </ul>
  </div>
</template>

<script>
import Container from "./components/Container.vue";
import data from "./assets/data.js";
import axios from "axios";
import { assertExpressionStatement } from "@babel/types";
export default {
  data() {
    return {
      postData: data,
      clickNum: 0,
      imgUrl: "",
      step: 0,
      postStr: "",
      choiceFilter: "",
    };
  },
  name: "App",
  components: {
    Container,
  },
  mounted() {
    this.emitter.on("filterChange", (a) => {
      this.choiceFilter = a;
    });
  },
  methods: {
    more() {
      axios
        .get(`https://codingapple1.github.io/vue/more${this.clickNum}.json`)
        .then((결과) => {
          this.postData.push(결과.data);
        });
      this.clickNum = this.clickNum == "0" ? "1" : "0";
    },
    fileUpload(e) {
      let file = e.target.files[0];
      this.imgUrl = URL.createObjectURL(file);
      this.step++;
    },
    clickUpload() {
      let newPost = {
        name: "Guest",
        userImage: "https://placeimg.com/100/100/arch",
        postImage: this.imgUrl,
        likes: 0,
        date: "May 15",
        liked: false,
        content: this.postStr,
        filter: "perpetua",
      };
      this.postData.unshift(newPost);
      this.step = 0;
    },
  },
};
</script>

<style>
body {
  margin: 0;
}
ul {
  padding: 5px;
  list-style-type: none;
}
.logo {
  width: 22px;
  margin: auto;
  display: block;
  position: absolute;
  left: 0;
  right: 0;
  top: 13px;
}
.header {
  width: 100%;
  height: 40px;
  background-color: white;
  padding-bottom: 8px;
  position: sticky;
  top: 0;
}
.header-button-left {
  color: skyblue;
  float: left;
  width: 50px;
  padding-left: 20px;
  cursor: pointer;
  margin-top: 10px;
}
.header-button-right {
  color: skyblue;
  float: right;
  width: 50px;
  cursor: pointer;
  margin-top: 10px;
}
.footer {
  width: 100%;
  position: sticky;
  bottom: 0;
  padding-bottom: 10px;
  background-color: white;
}
.footer-button-plus {
  width: 80px;
  margin: auto;
  text-align: center;
  cursor: pointer;
  font-size: 24px;
  padding-top: 12px;
}
.sample-box {
  width: 100%;
  height: 600px;
  background-color: bisque;
}
.inputfile {
  display: none;
}
.input-plus {
  cursor: pointer;
}
#app {
  box-sizing: border-box;
  font-family: "consolas";
  margin-top: 60px;
  width: 100%;
  max-width: 460px;
  margin: auto;
  position: relative;
  border-right: 1px solid #eee;
  border-left: 1px solid #eee;
}
</style>
