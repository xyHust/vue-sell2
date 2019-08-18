<template>
  <cube-scroll class="ratings" :data="computedRatings"  :options="scrollOptions">
    <div class="ratings-content">
      <div class="overview">
        <div class="overview-left">
          <h1 class="score">{{seller.score}}</h1>
          <div class="title">综合评分</div>
          <div class="rank">高于周边商家{{seller.rankRate}}%</div>
        </div>
        <div class="overview-right">
          <div class="score-wrapper">
            <span class="title">服务态度</span>
            <star :size="36" :score="seller.serviceScore"></star>
            <span class="score">{{seller.serviceScore}}</span>
          </div>
          <div class="score-wrapper">
            <span class="title">商品评分</span>
            <star :size="36" :score="seller.foodScore"></star>
            <span class="score">{{seller.foodScore}}</span>
          </div>
          <div class="delivery-wrapper" >
            <span class="title">送达时间</span>
            <span class="delivery">{{seller.deliveryTime}}分钟</span>
          </div>
        </div>
      </div>
      <split></split>
      <rating-select
        @select="onSelect"
        @toggle="onToggle"
        :selectType="selectType"
        :onlyContent="onlyContent"
        :ratings="ratings"
        v-if="ratings.length">

      </rating-select>
      <div class="rating-wrapper">
        <ul>
          <li
            v-for="(rating,index) in computedRatings"
            :key="index"
            class="rating-item border-bottom-1px">
            <div class="avatar">
              <img width="28" height="28" :src="rating.avatar">
            </div>
            <div class="content">
              <h1 class="name">{{rating.username}}</h1>
              <div class="star-wrapper">
                <star :size="24" :score="rating.score"></star>
                <span class="delivery" v-show="rating.deliveryTine">
                  {{rating.deliveryTime}}
                </span>
              </div>
              <p class="text">{{rating.text}}</p>
              <div class="recommend" v-show="rating.recommend&&rating.recommend.length"></div>
              <span class="icon-thumb_up"></span>
              <span
                class="item"
                v-for="(item,index) in rating.recommend"
                :key="index">
                {{item}}
              </span>
            </div>
            <div class="time">
              {{format(rating.rateType)}}
            </div>
          </li>
        </ul>
      </div>
    </div>
  </cube-scroll>
</template>

<script type="text/ecmascript-6">
  import { getRatings } from 'api'
  import Star from 'components/star/star'
  import Split from 'components/split/split'
  import moment from 'moment'
  import ratingMixin from 'common/mixins/rating'


  export default {
    name: 'ratings',
    mixins: [ratingMixin],
    props: {
      data: {
        type: Object
      }
    },
    data(){
      return {
        ratings: [],
        scrollOptions: {
          click: false,
          directionLockThreshold: 0
        }
      }
    },
    computed: {
      seller(){
        return this.data.seller||{}
      }
    },
    methods: {
      fetch() {
        if(!this.fetched){
          this.fetched = true
          getRatings({
            id: this.seller.id
          }).then((goods) => {
            this.goods = goods
          })
        }
      },
      format(time){
        return moment(time).format('YYYY-MM-DD hh:mm')
      }
    },
    components: {
      Star,
      Split
    }

  }
</script>

<style lang="stylus" rel="stylesheet/stylus">

</style>
