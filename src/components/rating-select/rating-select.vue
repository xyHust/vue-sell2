<template>
  <div class="rating-select">
    <div class="rating-type border-bottom-1px">
      <span @click="select(2)" class="block positive"
            :class="{'active': selectType===2}">
        {{desc.all}}<span class="count">{{ratings.length}}</span></span>
      <span @click="select(0)"  class="block positive"
            :class="{'active': selectType===0}">{{desc.positive}}<span
        class="count">{{positives.length}}</span></span>
      <span @click="select(1)" class="block negative"
            :class="{'active':selectType===1}">{{desc.negative}}<span
        class="count">{{negatives.length}}</span></span>
    </div>
    <div @click="toggle" class="switch" :class="{'on':onlyContent}">
      <span class="icon-check_circle"></span>
      <span class="text">只看有内容的评价</span>
    </div>
  </div>
</template>

<script type="text/ecmascript-6">
  const POSITIVE = 0
  const NEGATIVE = 1
  const ALL = 2
  const EVENT_SELECT = 'select'
  const EVENT_TOGGLE = 'toggle'

  export default {
    props: {
      ratings: {
        type: Array,
        default(){
          return []
        }
      },
      selectType: {
        type: Number,
        default: ALL
      },
      onlyContent: {
        type: Boolean,
        default: false
      },
      desc:{
        type: Object,
        default(){
          return {
            all: '全部',
            positive: '满意',
            negative: '不满意'
          }
        }
      }
    },
    computed: {
      positives(){
        return this.ratings.filter((rating)=>{
          return rating.rateType === POSITIVE
        })
      },
      negatives(){
        return this.ratings.filter((rating) => {
          return rating.rateType === NEGATIVE
        })
      }
    },
    methods: {
      selectType(type){
        this.$emit(EVENT_SELECT,type)
      },
      toggle(){
        this.$emit(EVENT_TOGGLE)
      }
    }
  }

</script>

<style lang="stylus" rel="stylesheet/stylus">

</style>
