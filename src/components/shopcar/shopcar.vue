<!-- 购物车 -->
<template>
  <div class="shopcar">
    <div class="content-left">
      <div class="logo-wrapper">
        <div class="logo" :class="{'high-light': totalCount > 0}">
          <i class="icon-shopping_cart"></i>
        </div>
        <div v-show="totalCount > 0" class="count" :class="{'space': totalCount > 9}">{{totalCount}}</div>
      </div>
      <div class="price" :class="{'high-light': totalPrice > 0}">￥{{totalPrice}}</div>
      <div class="desc">另需配送费￥{{deliveryPrice}}元</div>
    </div>
    <div class="content-right" :class="payClass">
      <div>{{payDesc}}</div>
    </div>
  </div>
</template>

<script type='text/ecmascript-6'>
export default {
  props: {
    selectFoods: {
      type: Array,
      default: function () {
        return [];
      }
    },
    deliveryPrice: {
      type: Number,
      default: 0
    },
    minPrice: {
      type: Number,
      default: 0
    }
  },
  computed: {
    totalPrice () {
      let totalPrice = 0;
      this.selectFoods.forEach(food => {
        totalPrice += food.price * food.count;
      });
      return totalPrice;
    },
    totalCount () {
      let totalCount = 0;
      this.selectFoods.forEach(food => {
        totalCount += food.count;
      });
      return totalCount;
    },
    payDesc () {
      if (this.totalPrice === 0) {
        return `￥${this.minPrice}元起送`;
      } else if (this.totalPrice < this.minPrice) {
        let diff = this.minPrice - this.totalPrice;
        return `还差￥${diff}元起送`;
      } else {
        return '去结算';
      }
    },
    payClass () {
      if (this.totalPrice < this.minPrice) {
        return 'not-enough';
      } else {
        return 'enough';
      }
    }
  }
};
</script>

<style lang='stylus' rel='stylesheet/stylus'>
.shopcar
  position fixed
  left 0
  bottom 0
  z-index 50
  display flex
  width 100%
  height 48px
  background #141d27
  align-items center
  .content-left
    flex 1
    font-size 0
    display flex
    align-items center
    .logo-wrapper
      position relative
      display inline-block
      margin -10px 12px 0 12px
      padding 6px
      width 56px
      height 56px
      box-sizing border-box
      border-radius 50%
      background #141d27
      .logo
        width 100%
        height 100%
        border-radius 50%
        background #2b343c
        display flex
        justify-content center
        align-items center
        color #80858a
        font-size 24px
        &.high-light
          background-color rgb(0, 160, 220)
          color #ffffff
      .count
        position absolute
        top 0
        right 0
        min-width 16px
        height 16px
        line-height 16px
        text-align center
        border-radius 16px
        font-size 12px
        font-weight 700
        color #ffffff
        background rgb(240, 20, 20)
        box-shadow 0 4px 8px 0 rgba(0, 0, 0, 0.4)
        &.space
          padding 0 4px
    .price
      display inline-block
      font-size 16px
      color rgba(255, 255, 255, 0.4)
      font-weight 700
      padding-right 12px
      margin-right 12px
      line-height 24px
      border-right 1px solid rgba(255, 255, 255, 0.1)
      &.high-light
        color #ffffff
    .desc
      display inline-block
      font-size 12px
      color rgba(255, 255, 255, 0.4)
  .content-right
    flex 0 0 105px
    height 100%
    background-color #2b333b
    display flex
    align-items center
    justify-content center
    font-size 14px
    color rgba(255, 255, 255, 0.4)
    font-weight 700
    &.not-enough
      background-color #2b333b
      font-size 12px
    &.enough
      background-color #00b43c
      color #ffffff
</style>
