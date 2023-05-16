<script>
export default {
  components: {

  },
  props: [],
  data() {
    return {
      city: null,
      temp: null,
      describe: null,
      img: null
    }
  },
  methods: {

    // 天氣資訊
    getInfo() {
      fetch("https://opendata.cwb.gov.tw/api/v1/rest/datastore/F-C0032-001?Authorization=CWB-6AD9E8C3-4C0C-423D-8A6D-BD4257220711&format=JSON")
        .then(res => res.json())
        .then(data => {
          console.log(data)
          this.city = data.records.location[6].locationName;

          this.temp = data.records.location[6].weatherElement[4].time[0].parameter.parameterName;

          this.describe = data.records.location[6].weatherElement[0].time[0].parameter.parameterName;
        })
    },

    // 圖片串流匯入
    logImg(e) {
      const sourceImg = e.target.files[0];

      // 圖片檔=>2進位置的格式(一大串字串)Base64
      const reader = new FileReader();
      reader.readAsDataURL(sourceImg);

      reader.onload = (e) => {
        // 賦予img圖片的值
        this.img = e.target.result;
        console.log(this.img);
      }
    }
  },
  mounted() {

  },
}
</script>

<template>
  <h1>{{ city }}</h1>
  <h2 v-if="temp !== null" :class="{ hot: temp > 28 }">{{ temp }}°C</h2>
  <h2>{{ describe }}</h2>
  <button type="button" @click="getInfo">GET</button>

  <!-- 單向數據流 (不能用v-model雙向綁定) -->
  <input type="file" @change="logImg" id="">

  <!-- 綁定圖片顯示 -->
  <img :src="img">
</template>

<style scoped>
.hot {
  color: red;
}
</style>
