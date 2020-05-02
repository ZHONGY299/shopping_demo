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
            <td><input type="text" v-model="g.price" @input="checkNumber(g.id)" @change="checkGoodsPrice(g)"/></td>
            <td>{{g.count*g.price}}</td>
            <td>
                <b-button variant="danger" @click="deleteByOne(g)">下架商品</b-button>
                <b-button variant="success" @click="updateGoods(g)">修改商品</b-button>
            </td>
        </tr>
        <tr>
            <td>上架商品总数：</td>
            <td>{{counts}}</td>
            <td>商品总价格</td>
            <td>{{sumPrice}}</td>
            <td><b-button variant="danger" @click="deleteAll()">删除所有商品</b-button></td>
            <td>
                <b-button variant="success" @click="queryGoodsPrice()">查询</b-button>
                <b-button variant="success" @click="addGoods()">添加</b-button>
            </td>
        </tr>
    </table>
    <div v-show="isUpdateshow">
      <table>
        <tr>
          <td>修改商品的编号(id)</td>
          <td><input type="text" v-model="updateGoodsObject.id" disabled></td>
        </tr>
        <tr>
          <td>修改的商品的名称(NAME)</td>
          <td><input type="text" v-model="updateGoodsObject.name"></td>
        </tr>
        <tr>
          <td>修改商品的库存量</td>
          <td><input type="text" v-model="updateGoodsObject.count" @blur="checkUpdateCount()" :class="{red:countisred}" @focus="countisred=false"></td>
        </tr>
        <tr>
          <td>修改商品的价格(price)</td>
          <td><input type="text" v-model="updateGoodsObject.price" @blur="checkUpdatePrice()" :class="{red:priceisred}" @focus="priceisred=false"></td>
        </tr>
        <tr>
          <td></td>
          <td><input type="submit" value="确认修改" @click="upDate()"></td>
        </tr>
      </table>
    </div>
    <div v-show="isAddshow">
      <table>
        <tr>
          <td>添加商品的编号(id)</td>
          <td><input type="text" v-model="addGoodsObject.id" disabled></td>
        </tr>
        <tr>
          <td>添加的商品的名称(NAME)</td>
          <td><input type="text" v-model="addGoodsObject.name"></td>
        </tr>
        <tr>
          <td>添加商品的库存量</td>
          <td><input type="text" v-model="addGoodsObject.count" :class="{red:countisred}"></td>
        </tr>
        <tr>
          <td>添加商品的价格(price)</td>
          <td><input type="text" v-model="addGoodsObject.price" :class="{red:priceisred}"></td>
        </tr>
        <tr>
          <td></td>
          <td><input type="submit" value="确认添加" @click="goodsAdd()"></td>
        </tr>
      </table>
    </div>
</div>
</template>

<script>
export default {
  data () {
    return {
      goodscounts: 0,
      isUpdateshow: false,
      isAddshow: false,
      priceisred: false,
      countisred: false,
      updateGoodsObject: {},
      addGoodsObject: {},
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
        g.price = 1.00
      }
    },
    deleteAll: function () {
      if (confirm('确认删除所有的数据吗？')) {
        this.goods = []
      }
    },
    deleteByOne: function (g) {
      if (confirm('确认下架这条数据吗？')) {
        // 返回不等于g的数组元素（相当于删除了g的元素）
        this.goods = this.goods.filter(item => (item !== g))
        // for (let t = 0; t < this.goods.length; t++) {
        //   // splice(t, 1)表示删除一整项
        //   this.goods.splice(t, 1)
        //   return
        // }
      }
    },
    queryGoodsPrice: function () {
      // 其中item是数组中元素，函数体是满足条件，返回所有满足条件之后新数组
      let goodsPrice = this.goods.filter(item => (item.price > 3000))
      console.log(goodsPrice)
      // let goodsPrice = []
      // for (let t = 0; t < this.goods.length; t++) {
      //   if (this.goods[t].price > 3000) {
      //     goodsPrice.push(this.goods[t])
      //     return
      //   }
      //   console.log(goodsPrice)
      // }
    },
    updateGoods: function (g) {
      // 双向数据绑定,同对象改变其中任意值另一个也跟着改变
      // this.updateGoodsObject = g

      // 双向数据绑定，不同对象数值类型相同，赋值
      this.updateGoodsObject = {}
      this.updateGoodsObject.id = g.id
      this.updateGoodsObject.name = g.name
      this.updateGoodsObject.count = g.count
      this.updateGoodsObject.price = g.price
      this.isUpdateshow = true
      this.isAddshow = false
      this.countisred = false
      this.priceisred = false
    },
    upDate: function () {
      // console.log(this.updateGoodsObject)
      if (confirm('确认修改以下数据吗？')) {
        var regu = /^[0-9]+\.?[0-9]*$/
        if (!regu.test(this.addGoodsObject.count)) {
          alert('请输入正确的商品价格(又浮点数或整数构成)')
          this.addGoodsObject.count = 1.00
          this.isred = true
          return
        } else {
          for (let t = 0; t < this.goods.length; t++) {
            if (this.goods[t].id === this.updateGoodsObject.id) {
              this.goods[t].id = this.updateGoodsObject.id
              this.goods[t].name = this.updateGoodsObject.name
              this.goods[t].count = this.updateGoodsObject.count
              this.goods[t].price = this.updateGoodsObject.price
              // console.log(this.goods[t].name)
              break
            }
          }
          this.isUpdateshow = false
        }
      }
      this.isUpdateshow = false
    },
    addGoods: function () {
      let max = this.goods[0].id
      for (let t = 0; t < this.goods.length; t++) {
        if (this.goods[t].id > max) {
          max = this.goods[t].id
        }
      }
      this.addGoodsObject.id = max + 1
      this.isAddshow = true
      this.isUpdateshow = false
    },
    goodsAdd: function () {
      // console.log(this.addGoodsObject)
      // this.goods.push(this.addGoodsObject)
      if (confirm('确认添加此条数据吗？')) {
        if (isNaN(this.addGoodsObject.count) || this.addGoodsObject.count < 0) {
          alert('请输入正确的商品个数如正整数')
          this.countisred = true
          return
        } else {
          this.countisred = false
        }
        var regu = /^[0-9]+\.?[0-9]*$/
        if (!regu.test(this.addGoodsObject.price)) {
          alert('请输入正确的商品价格(又浮点数或整数构成)')
          this.priceisred = true
          return
        } else {
          this.priceisred = false
        }
        this.countisred = false
        this.priceisred = false
        let g = {}
        g.id = this.addGoodsObject.id
        g.name = this.addGoodsObject.name
        g.count = this.addGoodsObject.count
        g.price = this.addGoodsObject.price
        g.price = parseFloat(g.price).toFixed(2)
        this.goods.push(g)
      } else {
        this.isAddshow = false
      }
      this.addGoodsObject = {}
      this.isAddshow = false
    },
    checkUpdateCount: function () {
      if (isNaN(this.updateGoodsObject.count) || this.updateGoodsObject.count < 0) {
        alert('请输入正确的商品个数如正整数')
        this.countisred = true
      } else {
        this.countisred = false
      }
    },
    checkUpdatePrice: function () {
      var regu = /^[0-9]+\.?[0-9]*$/
      if (!regu.test(this.updateGoodsObject.price)) {
        alert('请输入正确的商品价格(又浮点数或整数构成)')
        this.priceisred = true
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
      return sum.toFixed(2)
    }
  }
}
</script>
<style scoped>
/* @import url(); //引入CSS类 */
.red{
  border: 1px solid red;
}
.table tr:not(:first-child):not(:last-child):hover{
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
