<!-- goods -->
<template>
  <div class="goods">
    <div class="menu-wrapper" ref="menu">
      <ul>
        <li v-for="(item, index) in goods" :key="index" class="menu-li" :class="{'select': index === selectIndex}">
          <div class="menu-item" @click="_selectFoodItem(index)">
            <span class="icon" v-if="item.type > 0" :class="constants.classMap[item.type]"></span>
            <span class="text">{{item.name}}</span>
          </div>
        </li>
      </ul>
    </div>
    <div class="foods-wrapper" ref="foods">
      <ul>
        <li v-for="(item, index) in goods" :key="index" ref="foodItem">
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
              <div class="car-control-wrapper">
                <car-control :food="food"></car-control>
              </div>
            </li>
          </ul>
        </li>
      </ul>
    </div>
    <shopcar :delivery-price="seller.deliveryPrice" :min-price="seller.minPrice" :selectFoods="selectFoods"></shopcar>
  </div>
</template>

<script type='text/ecmascript-6'>

import constants from '../../common/js/constants.js';
import BScroll from 'better-scroll';
import shopcar from '../shopcar/shopcar';
import carControl from '../shopcar/carcontrol';

const ERR_OK = 0;

export default {
  props: {
    seller: Object
  },
  data () {
    return {
      goods: [],
      foodsHeightGroup: [],
      scrollY: 0
    };
  },
  created () {
    this.constants = constants;

    this.$http.get('/api/goods').then((response) => {
      response = response.body;
      if (response.errno === ERR_OK) {
        this.goods = response.data;

        this.$nextTick(() => {
          this._initScroll();
          this._calHeight();
        });
      }
    });
  },
  computed: {
    selectIndex () {
      for (let i = 0; i < this.foodsHeightGroup.length - 1; i++) {
        let start = this.foodsHeightGroup[i];
        let end = this.foodsHeightGroup[i + 1];
        if (this.scrollY >= start && this.scrollY < end) {
          return i;
        }
      }
      return 0;
    },
    selectFoods () {
      let selectFoods = [];
      this.goods.forEach(good => {
        good.foods.filter(food => food.count > 0).forEach(food => {
          selectFoods.push(food);
        });
      });
      return selectFoods;
    }
  },
  methods: {
    _initScroll () {
      this.menuScroll = new BScroll(this.$refs.menu, {
        scrollY: true,
        click: true
      });

      this.foodsScroll = new BScroll(this.$refs.foods, {
        scrollY: true,
        probeType: 3,
        click: true
      });

      this.foodsScroll.on('scroll', (pos) => {
        this.scrollY = Math.abs(pos.y);
      });
    },
    _calHeight () {
      let items = this.$refs.foodItem;
      let height = 0;
      this.foodsHeightGroup.push(height);
      for (let i = 0; i < items.length; i++) {
        height += items[i].clientHeight;
        this.foodsHeightGroup.push(height);
      }
    },
    _selectFoodItem (index) {
      let items = this.$refs.foodItem;
      this.foodsScroll.scrollToElement(items[index], 300);
    }
  },
  components: {
    shopcar,
    'car-control': carControl
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
      .menu-li
        divider-horizontal-line(1px, rgba(7, 17, 27, 0.1))
        &:after
          left 12px
          width 56px
        &:last-child
          &:after
            display none
        &.select
            background-color #fff
            font-weight 700
            margin-top -1px
            &:after
              display none
        .menu-item
          display table-cell
          vertical-align middle
          width 56px
          height 54px
          padding-left 12px
          padding-right 12px
          line-height 14px
          font-size 0px
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
        padding 18px
        divider-horizontal-line(1px, rgba(7, 17, 27, 0.1))
        &:after
          left 18px
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
            line-height 12px
          .extra
            margin-bottom 8px
            &:first-child
              margin-right 12px
          .price
            display flex
            align-items center
            font-weight 700
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
        .car-control-wrapper
          position absolute
          right 10px
          bottom 6px
</style>
