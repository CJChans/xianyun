<template>
    <div class="container">
        <script src="/qrcode.js"></script>
        <div class="main">
            <div class="pay-title">
                支付总金额 <span class="pay-price">￥ {{order.price}}</span>
            </div>
            <div class="pay-main">
                <h4>微信支付</h4>
                <el-row type="flex" 
                justify="space-between" 
                align="middle"
                class="pay-qrcode">
                    <div class="qrcode">
                        <!-- 二维码 -->
                        <!-- <canvas id="qrcode-stage"></canvas> -->
                        <div id="qrcode"></div>
                        <p>请使用微信扫一扫</p>
                        <p>扫描二维码支付</p>
                    </div>
                    <div class="pay-example">
                        <img src="http://157.122.54.189:9093/images/wx-sweep2.jpg" alt="">
                    </div>
                </el-row>
            </div>
        </div>
    </div>
</template>

<script>
export default {
    data(){
        return{
            //二维码页面详情
            order:{},
             // 定时器
            timer: null
        }
    },
    mounted(){
        // 订单id
        const {id} = this.$route.query;
        setTimeout(async()=>{
        // 请求订单详情
          const res =  await this.$axios({
            url: "/airorders/" + id,
            headers: {
                Authorization: `Bearer ${this.$store.state.user.userInfo.token}`
            }
        })
            console.log(res)
            this.order = res.data;
            new QRCode(document.getElementById("qrcode"), this.order.payInfo.code_url);

            //查询付款状态
            this.timer = setInterval(async() => {
                const res = await this.$axios({
                    url:"/airorders/checkpay",
                    method:"post",
                    headers: {
                            Authorization: `Bearer ${this.$store.state.user.userInfo.token}`
                        },
                    data:{
                        id:this.$route.query.id,
                        nonce_str:this.order.price,
                        out_trade_no:this.order.orderNo
                    }
                })
                console.log(res,66666666666)
                //获取支付状态
                const statusTxt = res.data.statusTxt
                //支付完成后判断
                if(statusTxt === "支付完成"){
                    this.$message.success(statusTxt)
                    clearInterval(this.timer)
                }
            }, 3000);

        },10)
    },
     // 组件销毁时候使用的，一般情况下用于清除定时器
    destroyed(){
        clearInterval(this.timer);
    }
}
</script>

<style scoped lang="less">
.container{
    background:#f5f5f5;
    padding: 30px 0;

    .main{
        width:1000px;
        margin:0 auto;

        .pay-title{
            text-align: right;
            span{
                font-size:28px;
                color:orangered;
            }
        }

        .pay-main{
            background:#fff;
            margin-top:10px;
            border-top: 5px orange solid;
            padding:30px;

            h4{
                font-size: 28px;
                font-weight: normal;
                margin-bottom: 10px;
            }

            .pay-qrcode{
                padding:0 80px;
            }

            .qrcode{
                border:1px #ddd solid;
                padding:15px;
                height: fit-content;

                #qrcode-stage{
                    width:200px;
                    height:200px;
                    margin-bottom: 10px;
                }

                p{
                    line-height: 2;
                    text-align: center;
                }
            }
        }
    }
}
</style>