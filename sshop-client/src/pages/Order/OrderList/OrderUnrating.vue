<template>
  <div class="order_doing">
    
    <div class="scrall_children">
      <div v-for="(orderItem, index) in ordersUnrating" :key="index">
        <OrderItem :orderItem="orderItem"></OrderItem>
        <div class="button">
          <el-button
            round
            :disabled="forbidden[index]"
            @click="showGood(orderItem)"
          >
            {{ forbidden[index] ? "已评价" : "商品点评" }}
          </el-button>
        </div>
        <el-divider class="my_divider"></el-divider>
      </div>
    </div>

<div class="makeRating">
      <MakeRating :order="order" ref="makeRating"/>

</div>





  </div>
</template>

<script>
import { mapState } from "vuex";
import BScroll from "better-scroll";
import { MessageBox, Toast } from "mint-ui";
import OrderItem from "../../../components/OrderItem/OrderItem.vue";
import MakeRating from "../../../components/MakeRating/MakeRating.vue"

export default {
  data() {
    return {
      forbidden: [], //代表订单是否禁用
      order: {},
    };
  },
  mounted() {
    this.$store.dispatch("getOrdersUnrating", () => {
      this._initScroll();
    });
  },
  computed: {
    ...mapState(["ordersUnrating"]),
  },
  methods: {
    _initScroll() {
      if (!this.scroll) {
        this.scroll = new BScroll(".order_doing", {
          probeType: 2, // 因为惯性滑动不会触发
          click: true,
        });
      } else {
        this.scroll.refresh();
      }
    },
    makeComments(orderid)   
     {

   },

      handleClose(done) {
        this.$confirm('确认关闭？')
          .then(_ => {
            done();
          })
          .catch(_ => {});
      },



          // 显示点击的food
    showGood (order) {
      // 设置要传递给food组件的数据
      this.order = order
      // 显示good组件 (在父组件中调用子组件对象的方法)
      this.$refs.makeRating.toggleShow()
    }




 },
  watch: {
    ordersUnrating() {
      if (this.ordersUnrating.length) {
   
        this._initScroll();
        for (let i = 0; i < this.ordersUnrating.length; i++) {
          this.forbidden.push(false);
        }
      }
    },
  },
  components: {
    OrderItem,
    MakeRating,
  },

};
</script>

<style lang="stylus" rel="stylesheet/stylus">
.my_divider {
  margin: 12px 0;
}

.order_doing {
  height: 100%;
}

.scrall_children {
  // margin-bottom: 500px;
}

.button {
  display: flex;
  justify-content: right;
  // right :10px
}

.scrall_children {
  background: #fff;
}
</style>
