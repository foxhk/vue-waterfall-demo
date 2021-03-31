<template>
  <div>
    <div class="head">
      <button @click="unshift">前面加</button>
      <button @click="push">后面加</button>
      <button @click="shuffle">洗牌</button>
    </div>
    <Waterfall
        :align="align"
        :line-gap="200"
        :min-line-gap="100"
        :max-line-gap="220"
        :single-max-width="300"
        :watch="items"
        @reflowed="reflowed"
        ref="waterfall"
    >
      <!-- each component is wrapped by a waterfall slot -->
      <WaterfallSlot
          v-for="(item, index) in items"
          :width="item.width"
          :height="item.height"
          :order="index"
          :key="item.index"
          move-class="item-move"
      >
        <div class="item" :style="item.style" :index="item.index"></div>
      </WaterfallSlot>
    </Waterfall>
  </div>
</template>

<script>
import Waterfall from "@/third/vue-waterfall/waterfall"
import WaterfallSlot from "@/third/vue-waterfall/waterfall-slot"

var ItemFactory = (function () {

  var lastIndex = 0

  function generateRandomItems(count) {
    var items = [], i
    for (i = 0; i < count; i++) {
      items[i] = {
        index: lastIndex++,
        style: {
          background: getRandomColor()
        },
        width: 100 + ~~(Math.random() * 50),
        height: 100 + ~~(Math.random() * 50)
      }
    }
    return items
  }

  function getRandomColor() {
    var colors = [
      'rgba(21,174,103,.5)',
      'rgba(245,163,59,.5)',
      'rgba(255,230,135,.5)',
      'rgba(194,217,78,.5)',
      'rgba(195,123,177,.5)',
      'rgba(125,205,244,.5)'
    ]
    return colors[~~(Math.random() * colors.length)]
  }

  return {
    get: generateRandomItems
  }

})()

export default {
  name: "VueWaterfallDemo",
  components: {
    Waterfall, WaterfallSlot,
  },
  data() {
    return {
      align: 'center',
      items: ItemFactory.get(10),
      isBusy: false
    }
  },
  methods: {
    unshift: function () {
      this.items.unshift.apply(this.items, ItemFactory.get(1))
    },
    push: function () {
      this.items.push.apply(this.items, ItemFactory.get(1))
    },
    shuffle: function () {
      this.items.sort(function () {
        return Math.random() - 0.5
      })
    },
    reflowed: function () {
      this.isBusy = false
    }
  }
}

// document.body.addEventListener('click', function () {
//   app.shuffle()
//   // app.$refs.waterfall.$emit('reflow') // manually trigger reflow action
// }, false)
//
// window.addEventListener('scroll', function () {
//   var scrollTop = document.documentElement.scrollTop || document.body.scrollTop
//   if (scrollTop + window.innerHeight >= document.body.clientHeight) {
//     app.addItems()
//   }
// })
</script>

<style scoped>
.item {
  position: absolute;
  top: 5px;
  left: 5px;
  right: 5px;
  bottom: 5px;
  font-size: 1.2em;
  color: rgb(0, 158, 107);
}

.item:after {
  content: attr(index);
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  -webkit-transform: translate(-50%, -50%);
  -ms-transform: translate(-50%, -50%);
}

.wf-transition {
  transition: opacity .3s ease;
  -webkit-transition: opacity .3s ease;
}

.wf-enter {
  opacity: 0;
}

.item-move {
  transition: all .5s cubic-bezier(.55, 0, .1, 1);
  -webkit-transition: all .5s cubic-bezier(.55, 0, .1, 1);
}
</style>
