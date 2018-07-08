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
    <div class="foods-wrapper">
      <ul>
        <li v-for="(item, index) in goods" :key="index">
          <h1 class="title">{{item.name}}</h1>
          <ul>
            <li class="food-item" v-for="(food, foodIndex) in item.foods" :key="foodIndex">
              <div class="icon">
                <img width="57" height="57" :src="food.icon" alt="icon">
              </div>
              <div class="content">
                <h2 class="name">{{food.name}}</h2>
                <p class="description">{{food.description}}</p>
                <div class="extra">
                  <span>月售{{food.sellCount}}份</span>
                  <span>好评率{{food.rating}}%</span>
                </div>
                <div class="price">
                  <span class="now">￥{{food.price}}</span>
                  <span class="old" v-if="!food.oldPrice">￥{{food.oldPrice}}0.00</span>
                </div>
              </div>
            </li>
          </ul>
        </li>
      </ul>
    </div>
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
        width 56px
        height 54px
        padding-left 12px
        line-height 14px
        font-size 0px
        divider-horizontal-line(1px, rgba(7, 17, 27, 0.1))
        &:after
          width 56px
          left 12px
        .icon
          tag-icon(12px, 3)
          margin-right 2px
        .text
          font-size 12px
          vertical-align top
    .foods-wrapper
      flex 1
      .title
        padding-left 14px
        height 26px
        line-height 26px
        border-left 2px solid #d9dde1
        font-size 12px
        color rgb(147, 153, 159)
        background-color #f3f5f7
      .food-item
        display flex
        flex-direction row
        margin 18px
        padding-bottom 18px
        divider-horizontal-line(1px, rgba(7, 17, 27, 0.1))
        &:last-child
          &:after
            display none
        .icon
          flex 0 0 57px
          margin-right 10px
        .content
          flex 1
          .name
            font-size 14px
            line-height 14px
            color rgb(7, 17, 27)
            margin-top 2px
            margin-bottom 8px
          .description, .extra
            font-size 10px
            line-height 10px
            color rgb(147, 153, 159)
          .description
            margin-bottom 8px
          .extra
            &:first-child
              margin-right 12px
          .price
            font-weight 700
            line-height 24px
            .now
              display inline-block
              font-size 14px
              color rgb(240, 20, 20)
              margin-right 8px
              &::first-letter
                font-size 10px
            .old
              font-size 10px
              color rgb(147, 153, 159)
              text-decoration line-through
              vertical-align top
</style>
