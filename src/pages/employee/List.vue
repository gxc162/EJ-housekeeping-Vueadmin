<template>
    <div>
        <el-button type="success" size="small" @click="toAddHandler">添加</el-button>
        <el-button type="danger" size="small">批量删除</el-button>
        <el-table :data="employees">
            <el-table-column prop="id" label="编号" fixed="left"></el-table-column>
            <el-table-column prop="username" label="用户名" fixed="left"></el-table-column>
            <el-table-column prop="realname" label="姓名"></el-table-column>
            <el-table-column prop="gender" label="性别"></el-table-column>
            <el-table-column prop="telephone" label="手机号"></el-table-column>
            <el-table-column prop="idCard" width="200" label="身份证号"></el-table-column>
            <el-table-column prop="bankCard" width="200" label="银行卡号"></el-table-column>
            <el-table-column label="操作" fixed="right">
        <template v-slot="slot">
          <a href="" @click.prevent="toDeleteHandler(slot.row.id)">删除</a>
          <a href="" @click.prevent="toUpdateHandler(slot.row)">修改</a>
        </template>
      </el-table-column>
        </el-table>
        <!-- 分页开始 -->
    <el-pagination layout="prev, pager, next" :total="50"></el-pagination>
    <!-- /分页结束 -->
    <!-- 模态框 -->
    <el-dialog
      :title="title"
      :visible.sync="visible"
      width="60%">
      测试：{{form}}
      <el-form :moduel="form" label-width="80px"> 
        <el-form-item label="用户名">
          <el-input v-model="form.username"/>
        </el-form-item>
        <el-form-item label="密码">
          <el-input type="password" v-model="form.password"/>
        </el-form-item>
        <el-form-item label="姓名">
          <el-input v-model="form.realname"/>
        </el-form-item>
        <el-form-item label="性别">
          <el-radio-group v-model="form.gender">
            <el-radio label="男">男</el-radio>
            <el-radio label="女">女</el-radio>
          </el-radio-group>
        </el-form-item>
        <el-form-item label="手机号">
          <el-input v-model="form.telephone"/>
        </el-form-item>
        <el-form-item label="身份证号">
          <el-input v-model="form.idCard"/>
        </el-form-item>
        <el-form-item label="银行卡号">
          <el-input v-model="form.bankCard"/>
        </el-form-item>
      </el-form>
      <span slot="footer" class="dialog-footer">
        <el-button size="small" @click="closeModalHandler">取 消</el-button>
        <el-button size="small" type="primary" @click="submitHandler">确 定</el-button>
      </span>
    </el-dialog>
    <!-- /模态框 -->
    </div>
</template>


<script>
import request from '@/utils/request'
import querystring from 'querystring'
export default {//暴露接口
    data(){
        return{
            title:"录入员工信息",
            visible:false,
            employees:[],
            form:{
              type:"waiter"

            }
        }
    },created(){
      this.loadData();
    },methods:{  
      submitHandler(){
        let url="http://localhost:6677/waiter/saveOrUpdate"
        //前端向后台发送请求，完成数据的保存操作
      request({
        url,
        method:"post",
        //告诉给后台我的请求体中放的是查询字符串
        headers:{
          "Content-Type":"application/x-www-form-urlencoded"
        },
        //请求体中的数据：将this.form转换为查询字符串的发送给后台
        data:querystring.stringify(this.form)
      }).then((response)=>{
        this.closeModalHandler();
        this.loadData();
        this.$message({
          type:"success",
          message:response.message
        })
      })
      },     
        loadData(){
          let url="http://localhost:6677/waiter/findAll"
          //this->vue实例，通过vue实例访问该实例中的数据，methods
          //this.title/this.toAddHandler
          request.get(url).then((response)=>{
            this.employees=response.data;
            //箭头函数中的this指回
          })
      
    },toAddHandler(){
            this.title="录入员工信息";
            this.visible=true;
        },
        toDeleteHandler(id){
            this.$confirm('此操作将永久删除该文件, 是否继续?', '提示', {
          confirmButtonText: '确定',
          cancelButtonText: '取消',
          type: 'warning'
        }).then(() => {
            //调用后台接口完成删除
            let url="http://localhost:6677/waiter/deleteById?id="+id;
            request.get(url).then((response)=>{
                //1.刷新数据
                this.loadData();
                //2.提示结果
                this.$message({
                type: 'success',
                message: response.message
                });
            })
          
        })
        },
    toUpdateHandler(row){
      this.title="修改员工信息";
      this.visible = true;
            this.form=row;

      
    },closeModalHandler(){
      this.visible = false;
    },
    }
}
</script>

<style scoped>

</style>