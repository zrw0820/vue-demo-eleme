<!-- star -->
<template>

<div class="star" v-bind:class="starType">
  <span class="star-item" v-for="(itemClass, index) in itemClasses" :class="itemClass" :key="index"></span>
</div>

</template>

<script type='text/ecmascript-6'>
const LENGTH = 5;
const CLS_ON = 'on';
const CLS_HALF = 'half';
const CLS_OFF = 'off';

export default {
  props: {
    size: Number,
    score: Number
  },
  computed: {
    starType () {
      return 'star-' + this.size;
    },
    itemClasses () {
      let result = [];
      let score = ~~this.score;
      let decimal = this.score - score;
      for (let i = 0; i < score; i++) {
        result.push(CLS_ON);
      }
      if (decimal >= 0.75) {
        result.push(CLS_ON);
        score += 1;
      } else if (decimal >= 0.25 && decimal < 0.75) {
        result.push(CLS_HALF);
        score += 1;
      }
      for (let i = 0; i < LENGTH - score; i++) {
        result.push(CLS_OFF);
      }
      return result;
    }
  }
};
</script>

<style lang='stylus' rel='stylesheet/stylus'>
  @import '../../common/stylus/mixin'

  .star
    font-size 0
    .star-item
      display inline-block
      background-repeat no-repeat
    &.star-24
      .star-item
        width 10px
        height 10px
        margin-right 3px
        background-size 10px 10px
        &:last-child
          margin-right 0
        &.on
          bg-image('star24_on')
        &.half
          bg-image('star24_half')
        &.off
          bg-image('star24_off')
    &.star-36
      .star-item
        width 15px
        height 15px
        margin-right 6px
        background-size 15px 15px
        &:last-child
          margin-right 0
        &.on
          bg-image('star36_on')
        &.half
          bg-image('star36_half')
        &.off
          bg-image('star36_off')
    &.star-48
      .star-item
        width 20px
        height 20px
        margin-right 22px
        background-size 20px 20px
        &:last-child
          margin-right 0
        &.on
          bg-image('star48_on')
        &.half
          bg-image('star48_half')
        &.off
          bg-image('star48_off')
</style>
