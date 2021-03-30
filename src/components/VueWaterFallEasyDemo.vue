<template>
  <div class="demo">
    <vue-waterfall-easy :imgsArr="imgsArr">
      <div class="head" slot="waterfall-head">
        <button @click="unshift">前面加</button>
        <button @click="push">后面加</button>
      </div>
      <div class="img-info" slot-scope="props">
        <p class="some-info">第{{props.index+1}}张图片</p>
        <p class="some-info">{{props.value.uid}}</p>
      </div>
    </vue-waterfall-easy>
  </div>
</template>

<script>
import axios from 'axios'
import vueWaterfallEasy from '../third/vue-waterfall-easy'
import guid from '@/util/guid'

const urlPrefix = "https://lfyfly.github.io/vue-waterfall-easy/demo";

export default {
  name: "WaterFallDemo",
  components: {
    vueWaterfallEasy
  },
  data() {
    return {
      data: require('../data.json'),
      imgsArr: [],
      group: 0, // request param
    }
  },
  created() {
    this.getData()
  },
  methods: {
    getData() {
      axios.get(urlPrefix + '/static/mock/data.json?group=' + this.group) // 真实环境中，后端会根据参数group返回新的图片数组，这里我用一个惊呆json文件模拟
          .then(res => {
            res.data.forEach(d => {
              d.src = urlPrefix + d.src.substr(1);
              d.uid = guid();
            });
            this.group++;
            this.imgsArr = this.imgsArr.concat(res.data)
          })
    },
    randomNew: function () {
      let idx = Math.floor(Math.random() * this.data.length) || 0;
      let d = JSON.parse(JSON.stringify(this.data[idx]));
      d.uid = guid();
      return d;
    },
    unshift() {
      this.imgsArr.unshift(this.randomNew());
    },
    push() {
      this.imgsArr.push(this.randomNew());
    }
  },
}
</script>

<style scoped>
 .demo {
   height: 600px;
 }
 .head {
   width: 100%;
   height: 20px;
   background: #f5fcae;
   box-shadow: 0 1px 3px rgba(0 0 0 0.3);
   margin-bottom: 5px;
 }
</style>
