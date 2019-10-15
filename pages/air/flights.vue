<template>
    <section class="contianer">
        <el-row  type="flex" justify="space-between">

            <!-- 顶部过滤列表 -->
            <div class="flights-content">
                <!-- 过滤条件 -->
                <div>
                    
                </div>
                
                <!-- 航班头部布局 -->
                <FlightsHeadList/>
                
                
                <!-- 航班信息 -->
                <FlightsItem v-for="(item,index) in dataList"
                :key="index"
                :item="item"/>

                <el-pagination
                v-if="flightsData.length>0"
                @size-change="handleSizeChange"
                @current-change="handleCurrentChange"
                :current-page="pageIndex"
                :page-sizes="[5, 10, 15, 20]"
                :page-size="pageSize"
                layout="total, sizes, prev, pager, next, jumper"
                :total="flightsData.total">
                </el-pagination>

                <div v-if="flightsData.length===0 && !loading" style="padding: 50px; text-align:center">
                    暂无该航班的数据   请重新选择
                </div>
            </div>

            <!-- 侧边栏 -->
            <div class="aside">
                <!-- 侧边栏组件 -->
            </div>
        </el-row>
    </section>
</template>

<script>

import FlightsHeadList from "@/components/air/flightsHeadList"
import FlightsItem from "@/components/air/flightsItem"

export default {
    components:{
        FlightsHeadList,
        FlightsItem
    },

    data(){
        return {
            // 请求机票列表返回的总数据，包含了flights,info, options,total
            flightsData:[],

             // 从flights总列表数据中切割出来数组列表
            dataList: [],

             // 当前的页数
            pageIndex: 1,
            // 当前的条数
            pageSize: 5,

            //判断是否在加载
            loading:true,
        }
    },

    mounted(){
        this.$axios({
            url:"/airs",
            //params是axios的get得参数
            params:this.$route.query
        }).then(res=>{
            console.log(res.data)
            const {flights} = res.data
            this.flightsData = flights
            //请求完毕
            this.loading = false;
             // 第一页的数据
            this.dataList = this.flightsData.slice(0, this.pageSize)
        })
    },

    methods:{
         // 分页条数切换时候触发, val是当前的条数
        handleSizeChange(val){
            //切换条数
            this.pageSize = val
            //重回第一条
             this.dataList = this.flightsData.slice(0, this.pageSize)

        },
        // 页数切换时候触发, val是当前的页数
        handleCurrentChange(val){
             // 修改当前的页数
            this.pageIndex = val;
            // 修改机票列表
            // 0, 5
            // 5, 10
            // 10, 15
            this.dataList = this.flightsData.slice(
                (this.pageIndex - 1 ) * this.pageSize,
                this.pageIndex * this.pageSize
            )
        }
    }
}
</script>

<style scoped lang="less">
    .contianer{
        width:1000px;
        margin:20px auto;
    }

    .flights-content{
        width:745px;
        font-size:14px;
    }

    .aside{
        width:240px;
    } 
</style>