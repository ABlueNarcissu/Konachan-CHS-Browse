<template>
  <!-- <RouterView /> -->
  <div class="content">
    <header class="header" v-if="status">Now Loading...</header>
    <img class="img" :src="urls[index]" @load="loading" @error="loading" />
    <footer class="footer">
      <div class="left">
        <input type="text" v-model="params" placeholder="以英文逗号分隔" />
        <input type="button" value="搜索" @click="search" />
      </div>
      <div class="right">
        <input type="button" value="上一张" @click="lastImg" />
        <input type="button" value="下一张" @click="nextImg" />
        <!-- <input type="button" value="刷新" @click="refresh" /> -->
      </div>
    </footer>
  </div>
</template>

<script setup lang="ts">
// import { RouterView } from "vue-router";
import axios from "axios";
import { ref, watch } from "vue";

let urls = ref([]);
let page = 1;
let tag = "rating:safe";
let index = ref(-1);
let status = ref(true);
let params = "";

watch(index, (newValue) => {
  if (newValue !== -1) {
    status.value = true;
  }
});

function loading() {
  status.value = false;
}

function getUrls(p: number) {
  axios.get(`/api/?tag=${tag}&p=${p}`).then((res) => {
    let findUlTag = /<ul.+?<\/ul>/;
    let ulTag = res.data.match(findUlTag)[0];
    // let findImageUrl =
    //   /ajaxs\('https:\/\/konachan\.net\/(sample|image).+?jpg/g;
    let findImageUrl = /,'https:\/\/konachan\.net\/image.+?(jpg|png)/g;
    let imageUrl = ulTag.match(findImageUrl);
    // let imageUrls = imageUrl.map((item: string) =>
    //   item.replace("ajaxs('", "")
    // );
    let imageUrls = imageUrl.map((item: string) => item.replace(",'", ""));
    urls.value = imageUrls;
    index.value = 0;
  });
}

function lastImg() {
  if (index.value > 0) {
    index.value--;
  } else if (index.value === 0 && page > 1) {
    page--;
    getUrls(page);
  }
}

function nextImg() {
  index.value++;
  if (index.value > 99) {
    index.value = -1;
    page++;
    getUrls(page);
  }
}

// function refresh() {
//   let img: any = document.querySelector(".img");
//   if (img["src"] !== "" && !status.value) {
//     status.value = true;
//     let random = getRandomIntInclusive(0, 10000);
//     img["src"] = img["src"] + `?${random}`;
//   }
// }

// function getRandomIntInclusive(min: number, max: number) {
//   min = Math.ceil(min);
//   max = Math.floor(max);
//   return Math.floor(Math.random() * (max - min + 1)) + min; //含最大值，含最小值
// }

document.addEventListener("keydown", (event) => {
  if (event.code === "ArrowRight" && index.value !== -1) {
    nextImg();
  }
});

function search() {
  tag = params.replace(",", "%20");
  status.value = true;
  getUrls(page);
}

getUrls(page);
</script>

<style>
body {
  margin: 0;
}

.content {
  height: 100vh;
  width: 100vw;
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  background-color: rgb(34, 34, 34);
  position: relative;
}

.header {
  background-color: hsla(0, 0%, 100%, 0.5);
  position: absolute;
  top: 0;
  padding: 10px 20px;
  box-sizing: border-box;
}

.img {
  max-height: 100vh;
  max-width: 100vw;
}

.footer {
  position: absolute;
  bottom: 0;
  width: 100vw;
  display: flex;
  justify-content: space-between;
  padding: 10px 20px;
  box-sizing: border-box;
}

.footer div input {
  margin: 0 6px;
}
</style>
