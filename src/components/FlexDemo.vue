<template>
  <div id="flex-demo" class="demo">
    <button v-on:click="add">添加</button>
    <transition-group name="flex-list" tag="div" class="container">
      <div class="box" v-for="item in items" v-bind:key="item.uid">
        <div> {{ item.info }}</div>
        <div>{{item.uid}}</div>
      </div>
    </transition-group>
  </div>
</template>

<script>
import guid from '@/util/guid'
export default {
name: "FlexDemo",
  data() {
    return {
      items: [],
    }
  },
  created() {
    let arr = require("../data.json");
    arr.forEach( d => {
      d.uid = guid();
      this.items.push(d);
    })
  },
  methods: {
    add() {
      let idx = Math.floor(Math.random() * this.items.length) || 0;
      let newObj = JSON.parse(JSON.stringify(this.items[idx])); // 深拷贝
      newObj.uid = guid();
      this.items.unshift(newObj);
    }
  }
}
</script>

<style scoped>
.demo {
  height: 600px;
  width: 100%;
}
.container {
  display: flex;
  flex-flow: row wrap;
  gap: 10px;
}
.flex-list-move {
  transition: transform 1s;
}
.box {
  width: 200px;
  height: 150px;
  border: 1px black solid;
  display: flex;
  flex-flow: column;
  justify-content: space-around;
  align-items: center;
}
</style>
