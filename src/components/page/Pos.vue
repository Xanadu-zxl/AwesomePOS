<template>
  <div class="pos">
    <el-row>
      <el-col :span="7" class="pos-order" id="order-list">
        <el-tabs>
          <el-tab-pane label="点餐">
            <el-table :data="tableData" border style="width:100%">
              <el-table-column prop="goodsName" label="商品名称"></el-table-column>
              <el-table-column prop="count" label="数量" width="60"></el-table-column>
              <el-table-column prop="price" label="金额" width="60"></el-table-column>
              <el-table-column label="操作" width="100" fixed="right">
                <template scope>
                  <el-button type="text" size="small" @click="delSingleGoods(scope.row)">删除</el-button>
                  <el-button type="text" size="small" @click="addOrderList(scope.row)">增加</el-button>
                </template>
              </el-table-column>
            </el-table>
            <div class="totalDiv">
              <span>数量：</span>
              {{totalCount}}
              <span>金额：</span>
              {{totalMoney}}元
            </div>
            <div class="divBtn">
              <el-button type="warning">下单</el-button>
              <el-button type="danger" @click="delAllGoods()">删除</el-button>
              <el-button type="success" @click="checkout">结账</el-button>
            </div>
          </el-tab-pane>
          <el-tab-pane label="下单">下单</el-tab-pane>
          <el-tab-pane label="外卖"></el-tab-pane>
        </el-tabs>
      </el-col>
      <el-col :span="17">
        <div class="often-goods">
          <div class="title">常用商品</div>
          <div class="often-goods-list">
            <ul>
              <li v-for="(goods,goodsId) in oftenGoods" :key="goodsId" @click="addOrderList(goods)">
                <span>{{goods.goodsName}}</span>
                <span class="o-price">￥{{goods.price}}元</span>
              </li>
            </ul>
          </div>
        </div>
        <div class="goods-type">
          <el-tabs>
            <el-tab-pane label="汉堡">
              <ul class="cookList">
                <li
                  v-for="(items,goodsId) in type0Goods"
                  :key="goodsId"
                  @click="addOrderList(goods)"
                >
                  <span class="foodImg">
                    <img :src="items.goodsImg" width="100%" />
                  </span>
                  <div class="food">
                    <span class="foodName">{{items.goodsName}}</span>
                    <span class="foodPrice">￥{{items.price}}.00元</span>
                  </div>
                </li>
              </ul>
            </el-tab-pane>
            <el-tab-pane label="小食">
              <ul class="cookList">
                <li
                  v-for="(items,goodsId) in type1Goods"
                  :key="goodsId"
                  @click="addOrderList(goods)"
                >
                  <span class="foodImg">
                    <img :src="items.goodsImg" width="100%" />
                  </span>
                  <div class="food">
                    <span class="foodName">{{items.goodsName}}</span>
                    <span class="foodPrice">￥{{items.price}}.00元</span>
                  </div>
                </li>
              </ul>
            </el-tab-pane>
            <el-tab-pane label="饮料">
              <ul class="cookList">
                <li
                  v-for="(items,goodsId) in type2Goods"
                  :key="goodsId"
                  @click="addOrderList(goods)"
                >
                  <span class="foodImg">
                    <img :src="items.goodsImg" width="100%" />
                  </span>
                  <div class="food">
                    <span class="foodName">{{items.goodsName}}</span>
                    <span class="foodPrice">￥{{items.price}}.00元</span>
                  </div>
                </li>
              </ul>
            </el-tab-pane>
            <el-tab-pane label="套餐">
              <ul class="cookList">
                <li
                  v-for="(items,goodsId) in type3Goods"
                  :key="goodsId"
                  @click="addOrderList(goods)"
                >
                  <span class="foodImg">
                    <img :src="items.goodsImg" width="100%" />
                  </span>
                  <div class="food">
                    <span class="foodName">{{items.goodsName}}</span>
                    <span class="foodPrice">￥{{items.price}}.00元</span>
                  </div>
                </li>
              </ul>
            </el-tab-pane>
          </el-tabs>
        </div>
      </el-col>
    </el-row>
  </div>
</template>

<script>
import axios from 'axios'

export default {
  name: 'Pos',
  data () {
    return {
      tableData: [],
      oftenGoods: [],
      type0Goods: [],
      type1Goods: [],
      type2Goods: [],
      type3Goods: [],
      totalMoney: 0,
      totalCount: 0
    }
  },
  created: function () {
    // eslint-disable-next-line no-unused-expressions
    axios
      .get(
        'https://www.easy-mock.com/mock/5b8b30dbf032f03c5e71de7f/kuaican/oftenGoods'
      )
      .then(reponse => {
        console.log(reponse)
        this.oftenGoods = reponse.data
      })
      .catch(error => {
        console.log(error)
        alert('网络错误，自己看着办！ ')
      })
    axios
      .get(
        'https://www.easy-mock.com/mock/5b8b30dbf032f03c5e71de7f/kuaican/typeGoods'
      )
      .then(response => {
        console.log(response)
        // this.oftenGoods=response.data;
        this.type0Goods = response.data[0]
        this.type1Goods = response.data[1]
        this.type2Goods = response.data[2]
        this.type3Goods = response.data[3]
      })
      .catch(error => {
        console.log(error)
        alert('网络错误，不能访问')
      })
  },
  methods: {
    // 添加订单列表的方法
    addOrderList (goods) {
      this.totalCount = 0 // 汇总数量清0
      this.totalMoney = 0
      let isHave = false
      // 判断是否这个商品已经存在于订单列表
      for (let i = 0; i < this.tableData.length; i++) {
        // console.log(this.tableData[i].goodsId)
        if (this.tableData[i].goodsId === goods.goodsId) {
          isHave = true // 存在
        }
      }
      // 根据isHave的值判断订单列表中是否已经有此商品
      if (isHave) {
        // 存在就进行数量添加
        let arr = this.tableData.filter(o => o.goodsId === goods.goodsId)
        arr[0].count++
        // console.log(arr)
      } else {
        // 不存在就推入数组
        let newGoods = {
          goodsId: goods.goodsId,
          goodsName: goods.goodsName,
          price: goods.price,
          count: 1
        }
        this.tableData.push(newGoods)
      }

      this.getAllMoney()

      // 进行数量和价格的汇总计算
      this.tableData.forEach(element => {
        this.totalCount += element.count
        this.totalMoney = this.totalMoney + element.price * element.count
      })
    },
    // 删除单个方法
    delSingleGoods (goods) {
      this.tableData = this.filter(o => o.goodsId !== goods.goodsId)
      this.getAllMoney()
    },

    // 删除全部方法
    delAllGoods () {
      this.tableData = []
      this.totalCount = 0
      this.totalMoney = 0
    },

    // 模拟结账
    checkout () {
      if (this.totalCount !== 0) {
        this.tableData = []
        this.totalCount = 0
        this.totalMoney = 0
        this.$message({
          message: '订单已生成，买买买是最开心的事情~',
          type: 'success'
        })
      } else {
        this.$message.error('怎么可以什么都没有呢？要不买个煎饼走吧！')
      }
    },
    // 汇总数量金额
    getAllMoney () {
      this.totalCount = 0
      this.totalMoney = 0
      if (this.tableData) {
        // 进行数量和价格的汇总计算
        this.tableData.forEach(element => {
          this.totalCount += element.count
          this.totalMoney = this.totalMoney + element.price * element.count
        })
      }
    }
  }
}
</script>

<style scoped>
.pos-order {
  background-color: aliceblue;
  border-radius: 1px solid #c0ccda;
}

.divBtn {
  margin-top: 10px;
}

.title {
  height: 20px;
  border-radius: 1px solid #d3dce6;
  background: #f9fafc;
  padding: 10px;
  text-align: left;
}

.often-goods-list ul li {
  list-style: none;
  float: left;
  background-color: antiquewhite;
  border-radius: 5px;
  border: 1px solid #e5e9f2;
  padding: 10px;
  margin: 10px;
  cursor: pointer;
}

.o-price {
  color: #58b7ff;
}

.goods-type {
  clear: both;
}
.cookList li {
  list-style: none;
  width: 10rem;
  border: 1px solid #e5e9f2;
  height: 3rem;
  overflow: hidden;
  background-color: #fff;
  padding: 2px;
  float: left;
  margin: 2px;
  display: flex;
  justify-content: center;
  align-items: center;
  cursor: pointer;
}

.foodImg {
  width: 3rem;
  height: 3rem;
}

.food {
  float: right;
  text-align: center;
  padding-left: 10px;
}
.foodName {
  display: block;
  font-size: 16px;
  color: brown;
}
.foodPrice {
  font-size: 12px;
}

.totalDiv {
  border-bottom: 1px solid #d3dce6;
  padding: 10px;
}
</style>
