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
                <FlightsItem v-for="(item,index) in flightsData"
                :key="index"
                :item="item"/>
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
            flightsData:[]
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
        })
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