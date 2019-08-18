<template>
  <transition name="fade">
    <cube-popup
      v-show="visible"
      :mask-closable=true
      :z-index=90
      position="bottom"
      type="shop-cart-list"
      @mask-click="maskClick"
    >
      <transition
        name="move"
        @after-leave="onLeave">
        <div v-show="visible">
          <h1 class="title">购物车</h1>
          <span @click="empty" class="empty">清空</span>
        </div>
        <cube-scroll class="list-content" ref="listContent">
          <ul>
            <li
              class="food"
              v-for="food in selectFoods"
              :key="food.name"
            >
              <span class="name">{{food.name}}</span>
              <div class="price">
                <span>￥{{food.price*food.count}}</span>
              </div>
              <div class="cart-control-wrapper">
                <cart-control  @add="onAdd" :food="food"></cart-control>
              </div>
            </li>
          </ul>
        </cube-scroll>
      </transition>

    </cube-popup>
  </transition>
</template>

<script type="text/ecmascript-6">
  import CartControl from 'components/cart-control/cart-control'
  import popupMixin from 'common/mixins/popup'


  const EVENT_HIDE = 'hide'

  const EVENT_LEAVE = 'leave'

  const EVENT_ADD = 'add'

  const EVENT_SHOW = 'show'

  export default{
    mixins: [popupMixin],
    name: 'shop-cart-list',
    props: {
      selectFoods: {
        type: Array,
        default(){
          return []
        }
      }
    },
    data(){
      return {
        visible: false
      }
    },
    created(){
      this.$on(EVENT_SHOW,() => {
        this.$nextTick(() => {
          this.$refs.listContent.refresh()
        })
      })
    },
    methods: {

      onLeave(){
        this.$emit(EVENT_LEAVE)
      },
      maskClick(){
        this.hide()
      },
      onAdd(target){
        this.$emit(EVENT_ADD,target)
      },
      empty(){
        this.dialogComp =  this.dialogComp||this.$createDialog({
            type: 'confirm',
            title: '清空购物车',
            content: '确认清空吗',
            $events: {
              confirm: () => {
                this.selectFoods.forEach((food) => {
                  food.count = 0
                })
                this.hide()
              }
            }
          })
        this.dialogComp.show()
      }
    },
    components: {
      CartControl
    }
  }

</script>

<style lang="stylus" rel="stylesheet/stylus">

</style>
