<template>
  <transition name="move" @aftre-leave="afterLeave">
    <div class="food" v-show="visible">
      <cube-scroll :data="computedRatings"   ref="scroll">
        <div class="food-content">
          <div class="image-header">
            <img :src="food.image">
            <div class="back" @click="hide">
              <i class="icon-arrow_lift"></i>
            </div>
          </div>
          <div class="content">
            <h1 class="title">{{food.name}}</h1>
            <div class="detail">
              <span class="sell-count">月售{{food.sellCount}}份</span>
              <span class="rating">好评率{{food.rating}}%</span>
            </div>
            <div class="price">
              <span class="now">￥{{food.price}}</span><span class="old"
              v-show="food.oldPrice">{{food.oldPrice}}</span>
            </div>
            <div class="cart-control-wrapper">
              <cart-control  @add="addFood" :food="food"></cart-control>
            </div>
            <transition name="fade">
              <div @click.stop="addFirst" class="buy" v-show="!food.count">
                加入购物车
              </div>
            </transition>
          </div>
          <split v-show="food.info"></split>
          <div class="info" v-show="food.info">
            <h1 class="title">商品信息</h1>
            <p class="text">{{food.info}}</p>
          </div>
          <split></split>
          <div class="rating">
            <h1 class="title">商品评价</h1>
            <rating-select
              @select="onSelect"
              @toggle="onToggle"
              :selectType="selectType"
              :onlyContent="onlyContent"
              :desc="desc"
              :ratings="ratings">
            </rating-select>
            <div class="rating-wrapper">
              <ul v-show="computedRatings&&computedRatings.length">
                <li
                  v-for="(rating,index) in computedRatings"
                  class="rating-item border-bottom-1px"
                  :key="index"
                >
                  <div class="user">
                    <span class="name">{{rating.username}}</span>
                    <img class="avatar" width="12" height="12" :src="rating.avator">
                  </div>
                  <div class="time">{{format(rating.rateTime)}}</div>
                  <p class="text">
                    <span :class="{'icon-thumb_up':rating.rateType===0,'icon-thumb_down': rating.rateType===1}"></span>
                    {{rating.text}}
                  </p>
                </li>
              </ul>
              <div class="no-rating" v-show="!computeRatings||!computedRatings.length">暂无评价</div>
            </div>
          </div>
        </div>
      </cube-scroll>
    </div>
  </transition>
</template>

<script type="text/ecmascript-6">
  import popupMixin from 'common/mixins/popup'
  import ratingMixin from 'common/mixins/rating'
  import Split from 'components/split/split'
  import CartControl from 'components/cart-control/cart-control'
  import RatingSelect from 'components/rating-select/rating-select'

  import moment from 'moment'


  const EVENT_SHOW = 'show'
  const EVENT_LEAVE = 'leave'
  const EVENT_ADD = 'add'



  export default {
    mixins: [popupMixin,ratingMixin],
    name: 'food',
    props: {
      food: {
        type: Object
      }
    },
    data(){
      return {
        desc: {
          all: '全部',
          positive: '推荐',
          negative: '吐槽'
        }
      }
    },
    computed: {
      ratings(){
        return this.food.ratings
      }
    },
    created(){
      this.$on(EVENT_SHOW,() => {
        this.$nextTick(() => {
          this.$refs.scroll.refresh()
        })
      })
    },
    methods: {
      afterLeave(){
        this.$emit(EVENT_LEAVE)
      },
      addFirst(event){
        this.$set(this.food,'count',1)
        this.$emit(EVENT_ADD,event.target)
      },
      addFood(target){
        this.$emit(EVENT_ADD,target)
      },
      format(time){
        return moment(time).format('YYYY-MM-DD hh:mm')
      }
    },
    components: {
      Split,
      CartControl,
      RatingSelect
    }
  }
</script>

<style lang="stylus" rel="stylesheet/stylus">

  @import "~common/stylus/variable"
  @import "~common/stylus/mixin.styl"

  .food
    position: fixed
    left : 0
    top: 0
    bottom : 48px
    z-index : 30
    width : 100%
    background: $color-white
    &.move-enter-active,&.move-leave-active
      transition: all 0.3s linear
    &.move-enter,&.move-leave-active
      transform : translate3d(100%,0,0)
    .image-header
      position :relative
      width: 100%
      height: 0

</style>
