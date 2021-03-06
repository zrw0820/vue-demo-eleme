<!-- header -->
<template>
<div class="header">
    <div class="content-wrapper">
      <div class="avatar">
        <img width="64" height="64" :src="seller.avatar" alt="avatar">
      </div>
      <div class="content">
        <div class="title">
          <span class="brand"></span>
          <span class="name">{{seller.name}}</span>
        </div>
        <div class="description">
          <span>{{seller.description}}/{{seller.deliveryTime}}分钟送达</span>
        </div>
        <div class="support" v-if="seller.supports">
          <span class="icon" v-bind:class="constants.classMap[seller.supports[0].type]"></span>
          <span class="text">{{seller.supports[0].description}}</span>
        </div>
      </div>
      <div class="support-count" v-if="seller.supports" v-on:click="showDetail">
        <span class="count">{{seller.supports.length}}个</span>
        <i class="icon-keyboard_arrow_right"></i>
      </div>
    </div>
    <div class="bulletin-wrapper" v-on:click="showDetail">
      <span class="bulletin-title"></span>
      <span class="bulletin-text">{{seller.bulletin}}</span>
      <i class="icon-keyboard_arrow_right"></i>
    </div>
    <div class="background">
      <img :src="seller.avatar" alt="background" width="100%" height="100%">
    </div>
    <transition name="detail-fade">
      <div class="detail" v-show="detailShow">
        <div class="detail-wrapper clearfix">
          <div class="detail-main">
            <h1 class="name">{{seller.name}}</h1>
            <div class="star-wrapper">
              <star :size="48" :score="seller.score"></star>
            </div>
            <div class="line">
              <text-line text="优惠信息"></text-line>
            </div>
            <ul class="supports" v-if="seller.supports">
              <li class="support-item" v-for="(item, index) in seller.supports" :key="index">
                <span class="icon" :class="constants.classMap[item.type]"></span>
                <span class="text">{{item.description}}</span>
              </li>
            </ul>
            <div class="line">
              <text-line text="商家公告"></text-line>
            </div>
            <div class="bulletin">
              <p class="content">{{seller.bulletin}}</p>
            </div>
          </div>
        </div>
        <div class="detail-close" v-on:click="hideDetail">
          <i class="icon-close"></i>
        </div>
      </div>
    </transition>
</div>
</template>

<script type='text/ecmascript-6'>
import star from '../star/star';
import textLine from '../common/textLine';
import constants from '../../common/js/constants.js';

export default {
  props: {
    seller: Object
  },
  components: {
    star, textLine
  },
  data () {
    return {
      detailShow: false
    };
  },
  created () {
    this.constants = constants;
  },
  methods: {
    showDetail () {
      this.detailShow = true;
    },
    hideDetail () {
      this.detailShow = false;
    }
  }
};

</script>

<style lang='stylus' rel='stylesheet/stylus'>
  @import '../../common/stylus/mixin'

  .header
    position relative
    overflow hidden
    color #fff
    background-color rgba(7, 17, 27, 0.5)
    .content-wrapper
      position relative
      padding 24px 12px 14px 24px
      font-size 0
      .avatar
        display inline-block
        vertical-align top
        img
          border-radius 2px
      .content
        display inline-block
        margin-left 16px
        .title
          margin 2px 0 8px 0
          .brand
            display inline-block
            vertical-align top
            width 30px
            height 18px
            bg-image('brand')
            background-size 30px 18px
            background-repeat no-repeat
          .name
            margin-left 6px
            font-size 16px
            line-height 18px
            font-weight bold
        .description
          margin-bottom 10px
          line-height 12px
          font-size 12px
        .support
          .icon
            tag-icon(12px, 1)
            margin-right 4px
          .text
            line-height 12px;
            font-size 10px
      .support-count
        position absolute
        right 12px
        bottom 18px
        padding 0 8px
        height 24px
        line-height 24px
        border-radius 14px
        background-color rgba(0, 0, 0, 0.2)
        text-align center
        .count
          font-size 10px
        .icon-keyboard_arrow_right
          vertical-align top
          margin-left 2px
          line-height 24px
          font-size 10px
    .bulletin-wrapper
      position relative
      height 28px
      line-height 28px
      padding 0 22px 0 12px
      white-space nowrap
      overflow hidden
      text-overflow ellipsis
      background-color rgba(7, 17, 27, 0.2)
      .bulletin-title
        display inline-block
        width 22px
        height 12px
        bg-image('bulletin')
        background-size 22px 12px
        background-repeat no-repeat
        font-size 10px
      .bulletin-text
        vertical-align top
        margin 0 4px
        font-size 10px
      .icon-keyboard_arrow_right
        position absolute
        right 12px
        top 10px
        font-size 10px
    .background
      position absolute
      top 0
      left 0
      width 100%
      height 100%
      z-index -1
      filter blur(10px)
    .detail
      position fixed
      z-index 100
      top 0
      left 0
      width 100%
      height 100%
      overflow auto
      background rgba(7, 17, 27, 0.8)
      -webkit-backdrop-filter blur(10px)
      .detail-wrapper
        width 100%
        min-height 100%
        .detail-main
          margin-top 64px
          margin-bottom 64px
          padding-bottom 10px
          .name
            line-height 16px
            text-align center
            font-size 16px
            font-weight 700
          .star-wrapper
            margin-top 18px
            padding 2px 0
            text-align center
          .line
            margin 28px 36px 24px 36px
          .supports
            margin 0 36px
            .support-item
              padding 0 12px 12px 12px
              font-size 0
              &:last-child
                padding-bottom 0
              .icon
                tag-icon(16px, 2)
                margin-right 6px
              .text
                line-height 16px
                font-size 12px
                font-weight 200
          .bulletin
            margin 0 36px
            .content
                padding 0 12px
                font-size 12px
                font-weight 200
                line-height 24px
      .detail-close
        position relative
        width 32px
        height 32px
        margin -64px auto 0 auto
        clear both
        font-size 32px
    .detail-fade-enter-active
    .detail-fade-leave-active {
      transition: opacity .5s;
    }
    .detail-fade-enter
    .detail-fade-leave-to {
      opacity: 0;
    }
</style>
