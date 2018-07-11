<!-- shop car add and sub control -->
<template>
  <div class="control">
    <transition name="move">
      <div class="sub" @click="sub()" v-show="food.count > 0">
        <i class="icon-remove_circle_outline"></i>
      </div>
    </transition>
    <transition name="move">
      <div class="count" v-show="food.count > 0">
        {{food.count}}
      </div>
    </transition>
    <div class="add" @click="add()">
      <i class="icon-add_circle"></i>
    </div>
  </div>
</template>

<script type='text/ecmascript-6'>
import Vue from 'vue';

export default {
  props: {
    food: Object
  },
  methods: {
    sub () {
      if (this.food.count > 0) {
        this.food.count--;
      }
    },
    add () {
      if (!this.food.count) {
        Vue.set(this.food, 'count', 1);
      } else {
        this.food.count++;
      }
    }
  }
};
</script>

<style lang='stylus' rel='stylesheet/stylus'>
  .control
    display flex
    align-items center
    font-size 0
    .sub, .add
      padding 6px
      font-size 24px
      color rgb(0, 160, 220)
    .count
      margin-bottom 3px
      font-size 10px
      color rgb(147, 153, 159)
    .move-enter-active, .move-leave-active
      transition: all 0.5s
    .move-enter, .move-leave-to
      opacity: 0
      transform translate3d(24px, 0, 0)
</style>
