<template>
<div class="container">
    <h1>购物车管理</h1>
    <table class="table table-hover">
        <tr>
            <td>商品编号</td>
            <td>商品名称</td>
            <td>商品库存</td>
            <td>商品价格</td>
            <td>该商品总价格</td>
            <td>操&nbsp;作</td>
        </tr>
        <tr v-for="g in goods" :key="g.id">
            <td>{{g.id}}</td>
            <td>{{g.name}}</td>
            <td>
                <b-button variant="info" @click="g.count++">+</b-button>
                <input type="number" v-model="g.count" @input="checkCountType(g)" min="1"/>
                <b-button variant="info" @click="checkCount(g)">-</b-button>
            </td>
            <td><input type="text" v-model="g.price" @blur="checkNumber(g.id)" @input="checkGoodsPrice(g)"/></td>
            <td>{{g.count*g.price}}</td>
            <td>
                <b-button variant="danger">下架商品</b-button>
                <b-button variant="success">修改商品</b-button>
            </td>
        </tr>
        <tr>
            <td>上架商品总数：</td>
            <td>{{counts}}</td>
            <td>商品总价格</td>
            <td>{{sumPrice}}</td>
            <td><b-button variant="danger">删除所有商品</b-button></td>
            <td>
                <b-button variant="success">添加</b-button>
            </td>
        </tr>
    </table>
</div>
</template>

<script>
export default {
  data () {
    return {
      goodscounts: 0,
      goods: [
        {
          id: 1001,
          name: 'iphone5',
          count: 2,
          price: 3500
        },
        {
          id: 1002,
          name: 'ipad',
          count: 4,
          price: 2000
        },
        {
          id: 1003,
          name: 'lenovo',
          count: 2,
          price: 5500
        },
        {
          id: 1004,
          name: '零食',
          count: 10,
          price: 100
        }

      ]
    }
  },
  methods: {
    checkNumber: function (gid) {
      for (let a = 0; a < this.goods.length; a++) {
        // console.log(this.goods[a])
        if (gid === this.goods[a].id) {
          // console.log(this.goods[a].price)
          if (isNaN(this.goods[a].price)) {
            alert('请输入一个数值！')
            this.goods[a].price = 0
            return
          }
        }
      }
    },
    checkCount: function (g) {
      if (g.count >= 2) {
        g.count = g.count - 1
      } else {
        alert('商品数量不能小于1！')
      }
    },
    checkCountType: function (g) {
      // alert(g.id)
      // if (!isNaN(g.count)) {
      //   g.count = 1
      // }
      if (!g.count) {
        g.count = 1
      }
    },
    checkGoodsPrice: function (g) {
      var regu = /^[0-9]+\.?[0-9]*$/
      if (!regu.test(g.price)) {
        alert('请输入正确的商品价格(又浮点数或整数构成)')
        g.price = 0.00
      }
    }
  },
  // 生命周期 --创建完成(访问当前this实例)
  created () {

  },
  // 生命周期 --挂载完成(访问DOM元素)
  mounted () {

  },
  computed: {
    counts: function () {
      let num = 0
      for (let a = 0; a < this.goods.length; a++) {
        num += parseInt(this.goods[a].count)
      }
      return num
    },
    sumPrice: function () {
      let num = 0
      let price = 0
      let sum = 0
      for (let a = 0; a < this.goods.length; a++) {
        num = parseInt(this.goods[a].count)
        price = parseFloat(this.goods[a].price)
        sum += num * price
      }
      return sum
    }
  }
}
</script>
<style scoped>
/* @import url(); //引入CSS类 */
tr:not(:first-child):not(:last-child):hover{
  background-color: #eee;
}
/* tr:hover{
  background-color: #eee;
}
tr:first-child{
  background-color: white;
}
tr:last-child{
  background-color: #fff;
} */
</style>
