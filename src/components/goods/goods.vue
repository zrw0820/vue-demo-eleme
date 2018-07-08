<!-- goods -->
<template>
  <div class="goods">
    <div class="menu-wrapper">
      <ul>
        <li v-for="(item, index) in goods" :key="index">
          <div class="menu-item">
            <span class="icon" v-if="item.type > 0" :class="constants.classMap[item.type]"></span>
            <span class="text">{{item.name}}</span>
          </div>
        </li>
      </ul>
    </div>
    <div class="foods-wrapper"></div>
  </div>
</template>

<script type='text/ecmascript-6'>

import constants from '../../common/js/constants.js';

const ERR_OK = 0;

export default {
  data () {
    return {
      goods: []
    };
  },
  created () {
    this.constants = constants;

    this.$http.get('/api/goods').then((response) => {
      response = response.body;
      if (response.errno === ERR_OK) {
        this.goods = response.data;
      }
    });
  }
};
</script>

<style lang='stylus' rel='stylesheet/stylus'>
  @import '../../common/stylus/mixin'

  .goods
    position absolute
    top 170px
    bottom 46px
    width 100%
    display flex
    flex-direction row
    overflow hidden
    .menu-wrapper
      width 80px
      background-color #f3f5f7
      .menu-item
        display table-cell
        vertical-align middle
        height 54px
        padding 0 12px
        line-height 10px
        font-size 0px
        position relative
        &:after
          position absolute
          width 56px
          border-top 1px solid rgba(7, 17, 27, 0.1)
          left 12px
          bottom 0
          content ''
        .icon
          tag-icon(12px, 3)
          margin-right 2px
        .text
          font-size 12px
    .foods-wrapper
      flex 1
</style>
