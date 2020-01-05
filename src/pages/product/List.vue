<template>
    <div>
        <div>
            <el-button size="small" type="primary" @click="toAddHandler">添加</el-button>
            <el-button size="small" type="danger" >批量删除</el-button>
        </div>
        <el-table :data="products">
            <el-table-column label="编号" prop='id'></el-table-column>
            <el-table-column label="产品名称" prop='name'></el-table-column>
            <el-table-column label="价格" prop='price'></el-table-column>
            <el-table-column label="描述" prop='description'></el-table-column>
            <el-table-column label="所属产品" prop='categoryId'></el-table-column>
            <el-table-column label="产品图片">
                <template slot-scope="scope">
                <img :src="scope.row.photo" width="100" height="100">
                </template>
            </el-table-column>

            <el-table-column label="操作"   fixed="right">
                <template v-slot='slot'>
                    <a href="" @click.prevent="toDeleteHandler(slot.row.id)">删除</a>
                    <a href="" @click.prevent="toUpdateHandler(slot.row)">修改</a>

                    <!-- {}显示脚本里面的数据 -->
                    <!-- {{slot.row.id}} -->
                </template>
            </el-table-column>
        </el-table>
        <!-- 分页 -->
        <el-pagination
            layout="prev, pager, next"
            :total="50">
        </el-pagination>
        <!-- 模态框 -->
        <el-dialog
            :title="title"
            :visible.sync="visible"
            width="40%">
            
            <el-form :model="form" label-width="80px" label-position="right" size="mini">
                <el-form-item label="产品名称">
                    <el-input  v-model="form.name"></el-input>
                </el-form-item>
                <el-form-item label="价格">
                    <el-input  v-model="form.price"></el-input>
                </el-form-item>
                <el-form-item label="所属栏目">
                    <el-select v-model="form.categoryId" placeholder="请选择">
                        <el-option
                        v-for="item in options"
                        :key="item.id"
                        :label="item.name"
                        :value="item.id">
                        </el-option>
                    </el-select>
                </el-form-item>
                <el-form-item label="描述">
                    <el-input  v-model="form.description"></el-input>
                </el-form-item>
                <el-form-item label="产品主图">
                    <el-upload
                            class="upload-demo"
                            action="http://134.175.154.93:6677/file/upload"
                            :file-list="fileList"
                            :on-success="uploadSuccessHandler"
                            list-type="picture">
                            <el-button size="small" type="primary">点击上传</el-button>
                            <div slot="tip" class="el-upload__tip">只能上传jpg/png文件，且不超过500kb</div>
                     </el-upload>
                </el-form-item>

            </el-form>
            <!-- footer---按钮操作区的内容 -->
            <span slot="footer" class="dialog-footer">
                <!-- @代表的事件的绑定 ==click事件-->
                <el-button size="small" @click="closeModalHandler">取 消</el-button>
                <el-button  size='small' type="primary" @click="submitHandler">确 定</el-button>
            </span>
        </el-dialog>
        <!-- 模态框结束 -->
    </div>
</template>
<script>
import request from '@/utils/request';//自定义库
import querystring from 'querystring';//系统库
export default {
    //用于存放要向网页中显示的数据
    data(){
        return{
            title:"录入产品信息",
            visible:false,
            // 动态模拟的数据
            products:[],
            fileList:[],
            options:[],
            form:{
            }        
        } 
    },
    created(){
        //在页面加载出来的时候加载数据
        this.loadData();
        //加载栏目信息，用于表单的下拉菜单
        this.loadCategory()
    },
methods:{
        uploadSuccessHandler(response){
            let photo = "http://134.175.154.93:8888/group1/"+response.data.id
            this.form.photo = photo;

            console.log(response);

        },
        submitHandler(){
            let url="http://localhost:6677/product/saveOrUpdate";
            //前端向后台发送请求，完成数据的保存操作
             request({
                 url,
                 method:"POST",
                 //告诉后台我的请求体中即将发送的是查询字符串
                 headers:{
                     "Content-Type":"application/x-www-form-urlencoded"
                 },
                 data: querystring.stringify(this.form)//转为查询字符串
             }).then((response)=>{
                 //请求结束后关闭模态框
                 this.visible=false;
                 //刷新页面
                 this.loadData();
                 //提示消息
                 this.$message({
                     type:"success",
                     message:response.message
                 })
             
      })
        },
        loadData(){
            // this>>Vue实例，通过vue实例访问该实例中的数据
            let  url="http://localhost:6677/product/findAll";
            request.get(url).then((response)=>{
                //this指向外部函数的this(url)。
                this.products=response.data;
            })
        },
        loadCategory(){
            // this>>Vue实例，通过vue实例访问该实例中的数据
            let  url="http://localhost:6677/category/findAll";
            request.get(url).then((response)=>{
                //this指向外部函数的this(url)。
                this.options=response.data;
            })
        },
         toDeleteHandler(id){
            this.$confirm('此操作将永久删除该文件, 是否继续?', '提示', {
            confirmButtonText: '确定',
            cancelButtonText: '取消',
             type: 'warning'
            }).then(() => {
                //调用后台接口完成删除操作
                let url="http://localhost:6677/product/deleteById?id="+id;
                request.get(url).then((response)=>{
                    //刷新数据，
                    this.loadData();
                    //提示结果
                     this.$message({
                        type: 'success',
                        message:response.message
            });
                })
            
            })
        },

        toUpdateHandler(row){
            this.title="修改产品信息"
            this.form=row;
            this.visible=true;
        },
        toAddHandler(){
            this.title="录入产品信息"
            this.visible=true;
        },
        closeModalHandler(){

            this.visible=false;
        }
    }
}
</script>>
<style scoped>

</style>
