<template>
    <div class="login_container">
        <div class="login_box">
            <div class="avatar_box">
                <img src="../assets/img/logo.png" alt="img">
            </div>
            <el-form :model="loginForm" ref="loginFormRef" :rules="loginFormRules" label-width="0px" class="login_form">
                <!-- 用户名 -->
                <el-form-item prop="username">
                    <el-input v-model="loginForm.username" prefix-icon="el-icon-user"></el-input>
                </el-form-item>
                <!-- 密码 -->
                <el-form-item prop="password">
                    <el-input v-model="loginForm.password" prefix-icon="el-icon-lock" type="password"></el-input>
                </el-form-item>
                <!-- 按钮 -->
                <el-form-item class="btns">
                    <el-button type="primary" @click="loginMethod">登录</el-button>
                    <el-button type="info" @click="resetLoginForm">重置</el-button>
                </el-form-item>
            </el-form>
        </div>
    </div>
</template>



<script>
export default {
    data(){
        return{
            loginForm:{
                username:"admin",
                password:"123456"
            },
            loginFormRules: {
                userName: [
                    { required: true, message: '请输入姓名', trigger: 'blur' },
                    { min: 3, max: 16, message: '长度在 3 到 16 个字符', trigger: 'blur' }
                ],
                password: [
                    { required: true, message: '请输入密码', trigger: 'blur' },
                    { min: 3, max: 16, message: '长度在 3 到 16 个字符', trigger: 'blur' }
                ]
            }
        }   
    },
    methods:{
        resetLoginForm(){
            this.$refs.loginFormRef.resetFields();
        },
        loginMethod(){
            this.$refs.loginFormRef.validate(async valid=>{
                if(!valid) return;
                const {data:res}=await this.$http.post("login",this.loginForm);
                switch(res.meta.status){
                    case 200:this.$message({message: '登录成功',type: 'success',center:true});break;
                    case 400:this.$message({message: '用户名或密码错误',type: 'error',center:true});return;break;
                    default:this.$message({message: '其他原因',type: 'error',center:true});return;
                }
                window.sessionStorage.setItem('token',res.data.token);
                this.$router.push('/home');
            });
        }
    }
}

</script>

<style lang="less" scoped>
    .login_container{
        background-color:rgb(48, 41, 94);
        height:100%;
    }
    .login_box{
        width:450px;
        height:300px;
        border-radius: 20px;
        background-color: white;
        position: absolute;
        top:50%;
        left:50%;
        transform:translate(-50%,-50%);
    }
    .avatar_box{
        width:130px;
        height:130px;
        border:1px solid grey;
        border-radius: 50%;
        padding :10px;
        box-shadow: 0 0 10px #ddd;
        background-color: white;
        transform: translate(-50%,-50%);
        position: absolute;
        left:50%;
        img{
            width:100%;
            height:100%;
            background-color: #eee;
            border-radius: 50%;
        }
    }
    .login_form{
        position:absolute;
        bottom:0px;
        width:100%;
        padding:0px 30px;
        box-sizing: border-box;
    }
    .btns{
        display: flex;
        justify-content: flex-end;
    }
</style>

