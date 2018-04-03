<template>
  <div class="pos">
    <div id="order-list">
      <el-row>
        <el-col :span="7">
          <el-tabs>
            <el-tab-pane label="点餐">
              <el-table :data="tableData" border style="width: 100%" >
                  <el-table-column prop="goodsName" label="商品"></el-table-column>
                  <el-table-column prop="count" label="量" width="50"></el-table-column>
                  <el-table-column prop="price" label="金额" width="70"></el-table-column>
                  <el-table-column  label="操作" width="100" fixed="right">
                      <template slot-scope="scope">
                          <el-button type="text" size="small" @click="delSingleGoods(scope.row)">删除</el-button>
                          <el-button type="text" size="small">增加</el-button>
                      </template>
                  </el-table-column>
              </el-table>
            </el-tab-pane>
            <!-- <el-tab-pane label="挂单">挂单</el-tab-pane>
            <el-tab-pane label="外卖">外卖</el-tab-pane> -->
          </el-tabs>
          <el-row type="flex" justify="center" class="goodsMsg">
            <el-col :span="6">数量：{{totalCount}}</el-col>
            <el-col :span="6">金额：{{totalMoney}}元</el-col>
          </el-row>
          <el-row type="flex" justify="center" style="margin-top:20px;">
              <el-button type="warning">挂单</el-button>
              <el-button type="danger" @click="delAllGoods()">删除</el-button>
              <el-button type="success" @click="checkout()">结账</el-button>
          </el-row>
        </el-col>
        <el-col :span="17">
          <div class="often-goods">
            <div class="title">常用商品</div>
            <div class="often-goods-list">
                <ul>
                    <li v-for="goods in oftenGoods" @click="addOrderList(goods)">
                        <span>{{goods.goodsName}}</span>
                        <span class="o-price">￥{{goods.price}}元</span>
                    </li>
                </ul>
            </div>
          </div>
          <div class="goods-type">
            <el-tabs>
              <el-tab-pane label="汉堡">
                <ul class='cookList'>
                    <li v-for="goods in type0Goods" @click="addOrderList(goods)">
                        <span class="foodImg"><img :src="goods.goodsImg" width="100%"></span>
                        <span class="foodName">{{goods.goodsName}}</span>
                        <span class="foodPrice">￥{{goods.price}}元</span>
                    </li>
                </ul>
              </el-tab-pane>
              <el-tab-pane label="小食">
                <ul class='cookList'>
                    <li v-for="goods in type1Goods" @click="addOrderList(goods)">
                        <span class="foodImg"><img :src="goods.goodsImg" width="100%"></span>
                        <span class="foodName">{{goods.goodsName}}</span>
                        <span class="foodPrice">￥{{goods.price}}元</span>
                    </li>
                </ul>
              </el-tab-pane>
              <el-tab-pane label="饮料">
                <ul class='cookList'>
                    <li v-for="goods in type2Goods" @click="addOrderList(goods)">
                        <span class="foodImg"><img :src="goods.goodsImg" width="100%"></span>
                        <span class="foodName">{{goods.goodsName}}</span>
                        <span class="foodPrice">￥{{goods.price}}元</span>
                    </li>
                </ul>
              </el-tab-pane>
              <el-tab-pane label="套餐">
                <ul class='cookList'>
                    <li v-for="goods in type3Goods" @click="addOrderList(goods)">
                        <span class="foodImg"><img :src="goods.goodsImg" width="100%"></span>
                        <span class="foodName">{{goods.goodsName}}</span>
                        <span class="foodPrice">￥{{goods.price}}元</span>
                    </li>
                </ul>
              </el-tab-pane>
            </el-tabs>
          </div>
        </el-col>
      </el-row>
    </div>
  </div>
</template>

<script>
import axios from 'axios'
export default {
  name: 'Pos',
  data () {
    return {
      tableData: [],
      oftenGoods: [
        {
          goodsId: 1,
          goodsName: "香辣鸡腿堡",
          price: 18
        },
        {
          goodsId: 2,
          goodsName: "田园鸡腿堡",
          price: 15
        },
        {
          goodsId: 3,
          goodsName: "和风汉堡",
          price: 15
        },
        {
          goodsId: 4,
          goodsName: "大包薯条",
          price: 18
        },
        {
          goodsId: 5,
          goodsName: "脆皮炸鸡腿",
          price: 20
        },
        {
          goodsId: 6,
          goodsName: "魔法鸡块",
          price: 20
        },
        {
          goodsId: 7,
          goodsName: "可乐大杯",
          price: 10
        },
        {
          goodsId: 8,
          goodsName: "雪顶咖啡",
          price: 18
        },
        {
          goodsId: 9,
          goodsName: "儿童欢乐套餐",
          price: 25
        },
        {
          goodsId: 10,
          goodsName: "快乐全家桶",
          price: 99
        }
      ],
      type0Goods: [
        {
            "goodsId": 1,
            "goodsImg": "http://7xjyw1.com1.z0.glb.clouddn.com/pos001.jpg",
            "goodsName": "香辣鸡腿堡",
            "price": 18
        },
        {
            "goodsId": 2,
            "goodsImg": "http://7xjyw1.com1.z0.glb.clouddn.com/pos002.jpg",
            "goodsName": "田园鸡腿堡",
            "price": 15
        },
        {
            "goodsId": 3,
            "goodsImg": "http://7xjyw1.com1.z0.glb.clouddn.com/pos003.jpg",
            "goodsName": "和风汉堡",
            "price": 15
        }
      ],
      type1Goods: [
        {
            "goodsId": 4,
            "goodsImg": "http://7xjyw1.com1.z0.glb.clouddn.com/pos003.jpg",
            "goodsName": "大包薯条",
            "price": 18
        },
        {
            "goodsId": 5,
            "goodsImg": "http://7xjyw1.com1.z0.glb.clouddn.com/pos002.jpg",
            "goodsName": "脆皮炸鸡腿",
            "price": 20
        },
        {
            "goodsId": 6,
            "goodsImg": "http://7xjyw1.com1.z0.glb.clouddn.com/pos001.jpg",
            "goodsName": "魔法鸡块",
            "price": 20
        }
      ],
      type2Goods: [
        {
            "goodsId": 7,
            "goodsImg": "http://7xjyw1.com1.z0.glb.clouddn.com/pos001.jpg",
            "goodsName": "可乐大杯",
            "price": 10
        },
        {
            "goodsId": 8,
            "goodsImg": "http://7xjyw1.com1.z0.glb.clouddn.com/pos002.jpg",
            "goodsName": "雪顶咖啡",
            "price": 18
        }
      ],
      type3Goods: [
        {
            "goodsId": 9,
            "goodsImg": "http://7xjyw1.com1.z0.glb.clouddn.com/pos004.jpg",
            "goodsName": "儿童欢乐套餐",
            "price": 25
        },
        {
            "goodsId": 10,
            "goodsImg": "http://7xjyw1.com1.z0.glb.clouddn.com/pos003.jpg",
            "goodsName": "快乐全家桶",
            "price": 99
        }
      ],
      totalCount: 0,
      totalMoney: 0,
    }
  },
  methods: {
    addOrderList(goods){
      let isHave = false;

      /*判断商品是否存在订单列表中*/
      for(let i = 0; i < this.tableData.length; i++){
        if(this.tableData[i].goodsId == goods.goodsId){
          isHave = true;
        }
      }

      if(isHave){
        /*存在就进行数量增加*/
        let arr = this.tableData.filter(o => o.goodsId == goods.goodsId);
        arr[0].count++;
      }else{
        /*不存在就加入数组*/
        let newGoods = {goodsId: goods.goodsId, goodsName:goods.goodsName, price: goods.price, count: 1};
        this.tableData.push(newGoods);
      }

      /*进行数量和总价的汇总计算*/
      this.getAllMoney();
    },
    delSingleGoods(goods){
      this.tableData = this.tableData.filter(o => o.goodsId != goods.goodsId)
      this.getAllMoney();
    },
    getAllMoney() {
      this.totalCount = 0;
      this.totalMoney = 0;
      if(this.tableData) {
        this.tableData.forEach((element) => {
          this.totalCount += element.count;
          this.totalMoney = this.totalMoney + (element.price * element.count);
        });
      }
    },
    delAllGoods() {
      this.tableData = [];
      this.totalCount = 0;
      this.totalMoney = 0;
    },
    checkout() {
      if (this.totalCount!=0) {
        this.tableData = [];
        this.totalCount = 0;
        this.totalMoney = 0;
        this.$message({
            message: '结账成功，感谢你又为店里出了一份力!',
            type: 'success'
        });
      }else{
        this.$message.error('不能空结。老板了解你急切的心情！');
      }
    }
  },
  mounted: function() {
    var orderHeight = document.body.clientHeight;
    document.getElementById("order-list").style.height = orderHeight + 'px';
  },
  created: function() {

    if(window.location.host == "zhjgh.github.io") return

    /*获取常用商品数据*/
    axios.get('http://jspang.com/DemoApi/oftenGoods.php')
    .then(res => {
      this.oftenGoods = res.data;
    })
    .catch(err => {
      console.log(err);
      alert('网络错误，不能访问');
    });

    /*获取分类商品数据*/
    axios.get('http://jspang.com/DemoApi/typeGoods.php')
    .then(res=>{
       this.type0Goods = res.data[0];
       this.type1Goods = res.data[1];
       this.type2Goods = res.data[2];
       this.type3Goods = res.data[3];
    })
    .catch(err=>{
        console.log(err);
        alert('网络错误，不能访问');
    });
  },
}
</script>

<style scoped>
.title{
  height: 20px;
  border-bottom:1px solid #D3DCE6;
  background-color: #F9FAFC;
  padding:10px;
}
.often-goods-list ul{
  overflow:hidden;
}
.often-goods-list ul li{
  list-style: none;
  float:left;
  border:1px solid #E5E9F2;
  padding:10px;
  margin:5px;
  background-color:#fff;
  cursor:pointer;
}
.o-price{
  color:#58B7FF;
}
.cookList li{
  list-style: none;
  width:23%;
  border:1px solid #E5E9F2;
  height: auto;
  overflow: hidden;
  background-color:#fff;
  padding: 2px;
  float:left;
  margin: 2px;
  cursor:pointer;
}
.cookList li span{
  display: block;
  float:left;
}
.foodImg{
  width: 40%;
}
.foodName{
  font-size: 18px;
  padding-left: 10px;
  color:brown;
}
.foodPrice{
  font-size: 16px;
  padding-left: 10px;
  padding-top:10px;
}
.goodsMsg{
  height:60px;
  line-height:60px;
  background-color:#fff;
}
</style>