<template>
  <div id="app">
    <div class="dynamic-component-demo">
      <button
          v-for="tab in tabs"
          v-bind:key="tab"
          v-bind:class="['tab-button', { active: currentTab === tab }]"
          v-on:click="currentTab = tab"
      >
        {{ tab }}
      </button>
      <!--      <keep-alive>-->
      <component v-bind:is="currentTabComponent" class="tab"></component>
      <!--      </keep-alive>-->
    </div>
  </div>

</template>

<script>
/**
 * ListCompleteDemo FlipListDemo SudokuDemo 来源 https://cn.vuejs.org/v2/guide/transitions.html
 * WaterFallDemo 来源 https://github.com/lfyfly/vue-waterfall-easy/blob/master/README-CN.md
 * VueAnimatedListDemo 来源 https://github.com/vuejs/vue-animated-list
 */
import ListCompleteDemo from "@/components/ListCompleteDemo";
import FlipListDemo from "@/components/FlipListDemo";
import SudokuDemo from "@/components/SudokuDemo";
import VueWaterFallEasyDemo from "@/components/VueWaterFallEasyDemo";
import StaggeredListDemo from "@/components/StaggeredListDemo";
import DynamicFadeDemo from "@/components/DynamicFadeDemo";
import FlexDemo from "@/components/FlexDemo";
import VueWaterfallDemo from "@/components/VueWaterfallDemo";

export default {
  name: 'App',
  components: {
    ListCompleteDemo, FlipListDemo, SudokuDemo, StaggeredListDemo, DynamicFadeDemo, VueWaterFallEasyDemo, FlexDemo, VueWaterfallDemo
  },
  data() {
    return {
      currentTab: "VueWaterfallDemo",
      tabs: ["ListCompleteDemo", "FlipListDemo", "SudokuDemo", "StaggeredListDemo", "DynamicFadeDemo",
        "VueWaterFallEasyDemo", "FlexDemo", "VueWaterfallDemo"],
    }
  },
  computed: {
    currentTabComponent: function () {
      return this.currentTab;
    }
  }
}
</script>

<style>
.tab-button {
  padding: 6px 10px;
  border-top-left-radius: 3px;
  border-top-right-radius: 3px;
  border: 1px solid #ccc;
  cursor: pointer;
  background: #f0f0f0;
  margin-bottom: -1px;
  margin-right: -1px;
}

.tab-button:hover {
  background: #e0e0e0;
}

.tab-button.active {
  background: #e0e0e0;
}

.tab {
  border: 1px solid #ccc;
  padding: 10px;
}
</style>
