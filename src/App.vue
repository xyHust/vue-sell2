<template>
  <div id="app">
    <v-header :seller="seller"></v-header>
    <div class="tab-wrapper">
      <tab :tabs="tabs"></tab>
    </div>

  </div>
</template>

<script type="text/ecmascript-6">
  import VHeader from 'components/v-header/v-header';
  import { getSeller } from 'api'
  import Goods from 'components/goods/goods'
  import Ratings from 'components/ratings/ratings'
  import Seller from 'components/seller/seller'
  import Tab from 'components/tab/tab'
  import qs from 'query-string'

  const ERR_OK = 0;

  export default {
    name: 'app',
    data() {
      return {
        seller: {
          id: qs.parse(location.search).id
        }
      };
    },
    computed: {
      tabs(){
        return [
          {
            label:'商品',
            component: Goods,
            data: {
              seller: this.seller
            }
          },
          {
            label:'评价',
            component: Ratings,
            data: {
              seller: this.seller
            }
          },
          {
            label:'商家',
            component: Seller,
            data: {
              seller: this.seller
            }
          }

        ]
      }
    },
    created() {
      this._getSeller()

    },
    methods:{
      _getSeller(){
        getSeller({
          id: this.seller.id
        }).then((seller) => {
          this.seller = seller
        })
      }
    },
    components: {
      Tab,
      VHeader
    }
  };
</script>

<style lang="stylus" rel="stylesheet/stylus">
  @import "./common/stylus/mixin.styl"
  .tab
    display: flex
    width: 100%
    height: 40px
    line-height: 40px
    border-1px(rgba(7, 17, 27, 0.1))
    .tab-item
      flex: 1
      text-align: center
      & > a
        display: block
        font-size: 14px
        color: rgb(77, 85, 93)
        &.active
          color: rgb(240, 20, 20)
</style>
