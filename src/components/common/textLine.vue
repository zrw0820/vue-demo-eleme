<!-- has text line -->
<template>
  <div class="text-line">{{text}}</div>
</template>

<script type='text/ecmascript-6'>
export default {
  props: {
    text: String
  }
};
</script>

<style lang='stylus' rel='stylesheet/stylus'>
  @import '../../common/stylus/mixin.styl'

  .text-line
    display flex
    flex-direction row
    align-items center
    font-size 14px
    color #fff
    font-weight 700
    line-height 14px
    &:before
    &:after
      margin-left 12px
      content ''
      flex 1
      height 1px
      background-color rgba(255, 255, 255, 0.2)
      scale-y()
    &:before
      margin-left 0
      margin-right 12px
</style>
