<template>
  <el-tabs v-model="activeName" @tab-click="handleClick">
    <el-tab-pane label="所有订单" name="first">
 <el-table :data="order">
<el-table-column prop="id" label="订单编号">  </el-table-column>
<el-table-column prop="orderTime" label="下单时间">  </el-table-column>
<el-table-column prop="total" label="总价"> </el-table-column>
<el-table-column prop="status" label="状态"> </el-table-column>
<el-table-column prop="customerId" label="顾客id"> </el-table-column>
<el-table-column prop="waiterId" label="员工id"> </el-table-column>
 <el-table-column label="操作">
               <template v-slot="slot">
                   <!-- <a href="" @click.prevent="toDeleteHandler(slot.row.id)">删除</a> -->
                   <!-- @click.prevent阻止默认行为 ----一般a标签用prevent-->
                   <!-- <a href="" @click.prevent="toUpdateHandler">修改</a> -->
               </template>
           </el-table-column>
 </el-table>
    </el-tab-pane>
    <el-tab-pane label="待支付" name="second">
<el-table :data="order">
<el-table-column prop="id" label="订单编号">  </el-table-column>
<el-table-column prop="orderTime" label="下单时间">  </el-table-column>
<el-table-column prop="total" label="总价"> </el-table-column>
<el-table-column prop="status" label="状态"> </el-table-column>
<el-table-column prop="customerId" label="顾客id"> </el-table-column>

 </el-table>

    </el-tab-pane>
    <el-tab-pane label="待派单" name="third">
<el-table :data="order">
<el-table-column prop="id" label="订单编号">  </el-table-column>
<el-table-column prop="orderTime" label="下单时间">  </el-table-column>
<el-table-column prop="total" label="总价"> </el-table-column>
<el-table-column prop="status" label="状态"> </el-table-column>
<el-table-column prop="customerId" label="顾客id"> </el-table-column>

 </el-table>

    </el-tab-pane>
    <el-tab-pane label="待接单" name="fourth">

<el-table :data="order">
<el-table-column prop="id" label="订单编号">  </el-table-column>
<el-table-column prop="orderTime" label="下单时间">  </el-table-column>
<el-table-column prop="total" label="总价"> </el-table-column>
<el-table-column prop="status" label="状态"> </el-table-column>
<el-table-column prop="customerId" label="顾客id"> </el-table-column>

 </el-table>
    </el-tab-pane>
    <el-tab-pane label="待服务" name="five">

<el-table :data="order">
<el-table-column prop="id" label="订单编号">  </el-table-column>
<el-table-column prop="orderTime" label="下单时间">  </el-table-column>
<el-table-column prop="total" label="总价"> </el-table-column>
<el-table-column prop="status" label="状态"> </el-table-column>
<el-table-column prop="customerId" label="顾客id"> </el-table-column>
<el-table-column prop="waiterId" label="员工id"> </el-table-column>
 </el-table>
    </el-tab-pane>
    <el-tab-pane label="待确认" name="six">

<el-table :data="order">
<el-table-column prop="id" label="订单编号">  </el-table-column>
<el-table-column prop="orderTime" label="下单时间">  </el-table-column>
<el-table-column prop="total" label="总价"> </el-table-column>
<el-table-column prop="status" label="状态"> </el-table-column>
<el-table-column prop="customerId" label="顾客id"> </el-table-column>
<el-table-column prop="waiterId" label="员工id"> </el-table-column>
 </el-table>
    </el-tab-pane>
    <el-tab-pane label="以完成" name="seven">

<el-table :data="order">
<el-table-column prop="id" label="订单编号">  </el-table-column>
<el-table-column prop="orderTime" label="下单时间">  </el-table-column>
<el-table-column prop="total" label="总价"> </el-table-column>
<el-table-column prop="status" label="状态"> </el-table-column>
<el-table-column prop="customerId" label="顾客id"> </el-table-column>
<el-table-column prop="waiterId" label="员工id"> </el-table-column>

 </el-table>
    </el-tab-pane>
  </el-tabs>
</template>
 <script>
import request from '@/utils/request';
import querystring from 'querystring';
export default { //用于存放网页中需要调用的方法ccc
    methods:{
        loadData(){
             let url = "http://localhost:6677/order/findAll"
        request.get(url).then((response)=>{
            //将查询结果设置到customers中,this指向外部函数的this
            this.order = response.data;
        })
        }},
 data(){
        return{
            visible: false,
            order:[],
            form:{ type:"order"}
        }
    },
    handleClick(){

    },
    created(){
      this.loadData();
    }
}
</script>
<style scoped>

</style>>