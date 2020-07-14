<template>
    <div>
        <el-button type="primary" @click="logout">退出</el-button>
    </div>
</template>

<script>
export default {
    methods:{
        logout(){
            window.sessionStorage.removeItem('token');
            window.sessionStorage.clear();
            this.$message({message: '退出成功',type: 'success',center:true})
            this.$router.push('/login');
        }
    }
}
</script>

<style lang="less" scoped>

</style>