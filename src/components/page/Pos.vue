<template>
  <div class="pos">
   <el-row>
     <el-col :span="7" class="pos-order" id="order-list">
       <el-tabs>
        <el-tab-pane label="点餐">
          <el-table :data="tableData" border show-summary style="width: 100%" >
              <el-table-column prop="goodsName" label="商品"  ></el-table-column>
              <el-table-column prop="count" label="数量" width="50"></el-table-column>
              <el-table-column prop="price" label="金额" width="70"></el-table-column>
              <el-table-column  label="操作" width="100" fixed="right">
                  <template scope="scope">
                      <el-button type="text" size="small" @click='delSingleGoods(scope.row)'>删除</el-button>
                      <el-button type="text" size="small" @click="addOrderList(scope.row)">增加</el-button>
                  </template>
              </el-table-column>
          </el-table> 
          <div class="totalDiv">
           <small> 数量：{{totalCount}}</small> &nbsp;&nbsp;&nbsp;&nbsp;<small> 金额: {{totalMoney}} 元</small>
          </div>
          <div class="div-btn">
           <el-button type="warning" >挂单</el-button>
            <el-button type="danger" @click="delAllGoods()">删除</el-button>
            <el-button type="success"@click="checkout()" >结账</el-button> 
          </div> 
          </el-tab-pane>
          <el-tab-pane label="挂单">
          挂单
          </el-tab-pane>
          <el-tab-pane label="外卖">
          外卖
        </el-tab-pane>
       </el-tabs>
     </el-col>
     <el-col :span="17">
       <div class="often-goods">
         <div class="title">常用商品</div>
          <div class="often-goods-list">
            <ul>
              <li v-for="(goods,index) in oftenGoods" :key='index' @click="addOrderList(goods)">
                <span>{{goods.goodsName}}</span>
                <span class="o-price">{{goods.price}}元</span>
              </li>
            </ul>
          </div>
       </div>
       <div class="goods-type">
         <el-tabs class="out-tab">
           <el-tab-pane label="汉堡">
             <div>
               <ul class="cookList">
                  <li v-for="(goods,index) in type0Goods" :key="index" @click="addOrderList(goods)">
                    <span class="foodImg"><img :src="goods.goodsImg" width="100%"></span>
                    <span class="foodName">{{goods.goodsName}}</span>
                    <span class="foodPrice">￥{{goods.price}}元</span>
               </li>
               </ul>
             </div>
           </el-tab-pane>
           <el-tab-pane label="小食">
             <div>
               <ul class="cookList">
                  <li v-for="(goods,index) in type1Goods" :key="index">
                    <span class="foodImg"><img :src="goods.goodsImg" width="100%"></span>
                    <span class="foodName">{{goods.goodsName}}</span>
                    <span class="foodPrice">￥{{goods.price}}元</span>
               </li>
               </ul>
             </div>
           </el-tab-pane>
           <el-tab-pane label="饮料">
             <div>
               <ul class="cookList">
                  <li v-for="(goods,index) in type2Goods" :key="index">
                    <span class="foodImg"><img :src="goods.goodsImg" width="100%"></span>
                    <span class="foodName">{{goods.goodsName}}</span>
                    <span class="foodPrice">￥{{goods.price}}元</span>
               </li>
               </ul>
             </div>
           </el-tab-pane>
           <el-tab-pane label="套餐">
             <div>
               <ul class="cookList">
                  <li v-for="(goods,index) in type3Goods" :key="index">
                    <span class="foodImg"><img :src="goods.goodsImg" width="100%"></span>
                    <span class="foodName">{{goods.goodsName}}</span>
                    <span class="foodPrice">￥{{goods.price}}元</span>
               </li>
               </ul>
             </div>
           </el-tab-pane>
         </el-tabs>
       </div>
     </el-col>
   </el-row>
  </div>
</template>

<script>
import axios from "axios";
export default {
  name: "Pos",
  data() {
    return {
      totalMoney: 0,
      totalCount: 0,
      tableData: [
        // {
        //   goodsName: '可口可乐',
        //   price: 8,
        //   count:1
        // }, {
        //   goodsName: '香辣鸡腿堡',
        //   price: 15,
        //   count:1
        // }, {
        //   goodsName: '爱心薯条',
        //   price: 8,
        //   count:1
        // }, {
        //   goodsName: '甜筒',
        //   price: 8,
        //   count:1
        // }
      ],
      oftenGoods: [
        //   {
        //       goodsId:1,
        //       goodsName:'香辣鸡腿堡',
        //       price:18
        //   }, {
        //       goodsId:2,
        //       goodsName:'田园鸡腿堡',
        //       price:15
        //   }, {
        //       goodsId:3,
        //       goodsName:'和风汉堡',
        //       price:15
        //   }, {
        //       goodsId:4,
        //       goodsName:'快乐全家桶',
        //       price:80
        //   }, {
        //       goodsId:5,
        //       goodsName:'脆皮炸鸡腿',
        //       price:10
        //   }, {
        //       goodsId:6,
        //       goodsName:'魔法鸡块',
        //       price:20
        //   }, {
        //       goodsId:7,
        //       goodsName:'可乐大杯',
        //       price:10
        //   }, {
        //       goodsId:8,
        //       goodsName:'雪顶咖啡',
        //       price:18
        //   }, {
        //       goodsId:9,
        //       goodsName:'大块鸡米花',
        //       price:15
        //   }, {
        //       goodsId:20,
        //       goodsName:'香脆鸡柳',
        //       price:17
        //   }
      ],
      type0Goods: [
        // {
        //     goodsId:1,
        //     goodsImg:"http://7xjyw1.com1.z0.glb.clouddn.com/pos001.jpg",
        //     goodsName:'香辣鸡腿堡',
        //     price:18
        // }, {
        //     goodsId:2,
        //     goodsImg:"http://7xjyw1.com1.z0.glb.clouddn.com/pos002.jpg",
        //     goodsName:'田园鸡腿堡',
        //     price:15
        // }, {
        //     goodsId:3,
        //     goodsImg:"http://7xjyw1.com1.z0.glb.clouddn.com/pos004.jpg",
        //     goodsName:'和风汉堡',
        //     price:15
        // }, {
        //     goodsId:4,
        //      goodsImg:"http://7xjyw1.com1.z0.glb.clouddn.com/pos003.jpg",
        //     goodsName:'快乐全家桶',
        //     price:80
        // }, {
        //     goodsId:5,
        //      goodsImg:"http://7xjyw1.com1.z0.glb.clouddn.com/pos003.jpg",
        //     goodsName:'脆皮炸鸡腿',
        //     price:10
        // }, {
        //     goodsId:6,
        //      goodsImg:"http://7xjyw1.com1.z0.glb.clouddn.com/pos004.jpg",
        //     goodsName:'魔法鸡块',
        //     price:20
        // }, {
        //     goodsId:7,
        //      goodsImg:"http://7xjyw1.com1.z0.glb.clouddn.com/pos001.jpg",
        //     goodsName:'可乐大杯',
        //     price:10
        // }, {
        //     goodsId:8,
        //      goodsImg:"http://7xjyw1.com1.z0.glb.clouddn.com/pos003.jpg",
        //     goodsName:'雪顶咖啡',
        //     price:18
        // }, {
        //     goodsId:9,
        //      goodsImg:"http://7xjyw1.com1.z0.glb.clouddn.com/pos002.jpg",
        //     goodsName:'大块鸡米花',
        //     price:15
        // }, {
        //     goodsId:20,
        //      goodsImg:"http://7xjyw1.com1.z0.glb.clouddn.com/pos002.jpg",
        //     goodsName:'香脆鸡柳',
        //     price:17
        // }
      ],
      type1Goods: [],
      type2Goods: [],
      type3Goods: []
    };
  },
  created: function() {
    axios
      .get(
        "https://www.easy-mock.com/mock/5b8b30dbf032f03c5e71de7f/kuaican/oftenGoods"
      )
      .then(resopnse => {
        // console.log(resopnse)
        this.oftenGoods = resopnse.data;
      })
      .catch(error => {
        alert("网络错误，不能访问");
      });

    axios
      .get(
        "https://www.easy-mock.com/mock/5b8b30dbf032f03c5e71de7f/kuaican/typeGoods"
      )
      .then(Response => {
        console.log(Response);
        this.type0Goods = Response.data[0];
        this.type1Goods = Response.data[1];
        this.type2Goods = Response.data[2];
        this.type3Goods = Response.data[3];
      })
      .catch();
  },
  mounted: function() {
    var orderHeight = document.body.clientHeight;
    console.log(orderHeight);
    document.getElementById("order-list").style.height = orderHeight + "px";
  },
  methods: {
    addOrderList(goods) {
      this.totalMoney = 0;
      this.totalCount = 0;
      //商品是否存在订单列表中
      let isHave = false;
      for (let i = 0; i < this.tableData.length; i++) {
        if (this.tableData[i].goodsId == goods.goodsId) {
          isHave = true;
        }
      }
      //根据判断的值编写业务逻辑
      if (isHave) {
        //改变列表商品的数量,如果该商品已经加入了  在继续加就根据id值筛选让其count数加1
        let arr = this.tableData.filter(o => o.goodsId == goods.goodsId);
        arr[0].count++;
      } else {
        let newGoods = {
          goodsId: goods.goodsId,
          goodsName: goods.goodsName,
          price: goods.price,
          count: 1
        };
        this.tableData.push(newGoods);
      }
      this.getAllMoney();
    },
    //删除单个商品
    delSingleGoods(goods) {
      //筛选出id不相等的
      this.tableData = this.tableData.filter(o => o.goodsId != goods.goodsId);
      console.log(this.tableData);
      this.getAllMoney();
    },
    //汇总数量和金额
    getAllMoney() {
      this.totalCount = 0;
      this.totalMoney = 0;
      if (this.tableData) {
        //计算数量和总金额
        this.tableData.forEach(element => {
          this.totalCount += element.count;
          this.totalMoney = this.totalMoney + element.price * element.count;
        });
      }
    },
    //删除全部
    delAllGoods() {
      this.tableData = [];
      this.totalMoney = 0;
      this.totalCount = 0;
    },
    //模拟结账
    checkout() {
      if (this.totalCount != 0) {
        this.tableData = [];
        this.totalMoney = 0;
        this.totalCount = 0;
        this.$message({
          message: "结账成功，感谢你又为店里做出了一份力",
          type: "checkoutsuccess"
        });
      } else {
        this.$message.error("不能空结，老板了解你急切的心情");
      }
    }
  }
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.pos-order {
  background-color: #f9fafc;
  border-right: 1px solid #c0ccda;
}
.div-btn {
  margin-top: 10px;
}
.title {
  height: 20px;
  border-bottom: 1px solid #d3dce6;
  background-color: #f9fafc;
  padding: 10px;
  text-align: left;
}
.totalDiv {
  background: #fff;
  padding: 10px;
  border-bottom: 1px solid #d3dce6;
}
.often-goods-list ul li {
  list-style: none;
  float: left;
  border: 1px solid #e5e911;
  padding: 10px;
  margin: 10px;
  background: #fff;
}
.o-price {
  color: #58b7ff;
}
.goods-type {
  clear: both;
}
.out-tab {
  margin-left: 15px;
}
.cookList li {
  list-style: none;
  width: 23%;
  border: 1px solid #e5e9f2;
  height: auot;
  overflow: hidden;
  background-color: #fff;
  padding: 2px;
  float: left;
  margin: 2px;
}
.cookList li span {
  display: block;
  float: left;
}
.foodImg {
  width: 40%;
}
.foodName {
  font-size: 16px;
  padding-left: 10px;
  color: brown;
}
.foodPrice {
  font-size: 16px;
  padding-left: 10px;
  padding-top: 10px;
}
</style>
