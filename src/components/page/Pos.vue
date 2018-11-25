<template>
  <div class="pos">
   <el-row>
     <el-col :span="7" class="pos-order" id="order-list">
       我是订单栏
     </el-col>
     <el-col>
       我是产品栏
     </el-col>
   </el-row>
  </div>
</template>

<script>
export default {
  name: 'Pos',
  mounted:function(){
var orderHeight = document.body.clientHeight;
console.log(orderHeight);
document.getElementById('order-list').style.height = orderHeight +'px'
  },
  data () {
    return {
     
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
 .pos-order{
   background-color:#f9fafc;
   border-right: 1px solid #c0ccda;
 }
</style>
