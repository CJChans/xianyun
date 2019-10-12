<template>
    <el-form 
        :model="form" 
        ref="form" 
        :rules="rules" 
        class="form">
            <el-form-item class="form-item" prop="username">
                <el-input 
                placeholder="用户名手机"
                v-model="form.username">
                </el-input>
            </el-form-item>

            <el-form-item class="form-item" prop="captcha">
                <el-input 
                placeholder="验证码"
                v-model="form.captcha" >
                    <template slot="append">
                        <el-button @click="handleSendCaptcha">
                            发送验证码
                        </el-button>
                    </template>
                </el-input>
            </el-form-item>

            <el-form-item class="form-item" prop="nickname">
                <el-input 
                placeholder="你的名字"
                v-model="form.nickname">
                </el-input>
            </el-form-item>

            <el-form-item class="form-item" prop="password">
                <el-input 
                placeholder="密码"
                v-model="form.password" 
                type="password"
                ></el-input>
            </el-form-item>

            <el-form-item class="form-item" prop="checkPassword">
                <el-input 
                placeholder="确认密码"
                v-model="form.checkPassword" 
                type="password">
                </el-input>
            </el-form-item>

            <el-button 
            class="submit" 
            type="primary" 
            @click="handleRegSubmit">
                注册
            </el-button>
        </el-form>
</template>

<script>
export default {
    data(){
        var validatePass = (rule, value, callback) => {
        if (value === '') {
            // new Error js原生的错误对象
          callback(new Error('请再次输入密码'));
        } else if(value!=this.form.password) {
             callback(new Error('两次输入密码不一致!'));
        }else{
            // 验证通过
            callback()
        }
      };
        return {
            // 表单数据
            form: {
                username:"",
                nickname:"",
                captcha:"",
                password:"",
                checkPassword:""
            },
            // 表单规则
            rules: {
                 username: [
                    { required: true, message: '请输入用户名', trigger: 'blur' }
                ],
                nickname: [
                    { required: true, message: '请输入昵称', trigger: 'blur' }
                ],
                captcha: [
                    { required: true, message: '请输入验证码', trigger: 'blur' }
                ],
                password: [
                    { required: true, message: '请输入密码', trigger: 'blur' }
                ],
                checkPassword: [
                    { validator: validatePass, trigger: 'blur' }
                ],
            },
        }
    },
    methods: {
        // 发送验证码
        async handleSendCaptcha(){
            if(!this.form.username){
                this.$message.error("请输入用户名手机")
                return;
            }
            const res=await this.$axios({
                url:"/captchas",
                method:"post",
                data:{
                    tel:this.form.username
                }
            });
            // console.log(res.data)
            const {data} = res;
            this.$message.success(`当前的手机验证码为：${data.code}`)
        },


        // 注册
        handleRegSubmit(){
        //    console.log(this.form)
           this.$refs.form.validate(async valid => {
            if (valid) {
                // props是form里面除了checkPassword以外的属性
                  const {checkPassword, ...props} = this.form
                  const res = await this.$axios({
                      url:"/accounts/register",
                      method:"post",
                      data:props                     
                  });
                  console.log(res)
                if(res.status===200){
                     this.$message.success("恭喜你注册成功")
                     //跳转到首页
                     this.$router.push("/")

                     const data = res.data;
                       // 把用户信息token保存到本地，在头部组件中显示用户数据
                       
                       // vuex不能通过直接赋值方式来修改state的值
                       // this.$store.state.user.username = data.user.nickname;
                       // 通过调用mutation下的方法掉修改state的值,commit方法调用mutation的方法
                       // 非常类似于$emit
                       this.$store.commit("user/setUserInfo", data);
                }
            }
        });
        }
    }
}
</script>

<style scoped lang="less">
    .form{
        padding:25px;
    }

    .form-item{
        margin-bottom:20px;
    }

    .form-text{
        font-size:12px;
        color:#409EFF;
        text-align: right;
        line-height: 1;
    }

    .submit{
        width:100%;
        margin-top:10px;
    }
</style>