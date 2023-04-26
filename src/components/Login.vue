<template>
    <div class="login-page">
        <div class="login-box" v-if="showLogin">
            <h3 style="margin-bottom: 20px;">登录</h3>
            <el-form>
                <el-form-item label="用户名" label-width="60px">
                    <el-input v-model="username" />
                </el-form-item>
                <el-form-item label="密码" label-width="60px">
                    <el-input v-model="password"  type="password" />
                </el-form-item>

            </el-form>
            <div style="width: 60px;">
                <el-button type="primary" @click='login'>登录</el-button>
            </div>
        </div>
        <Message v-else :token='token' />
    </div>
</template>

<script setup lang="ts">
import { ref } from 'vue';
//@ts-ignore
import { ElMessage } from 'element-plus';
import 'element-plus/theme-chalk/el-message-box.css'
import Message from './Message.vue'
import axios from 'axios'

const username = ref('')
const password = ref('')
const showLogin = ref(true)
let token = ''
function login() {
    //   showLogin.value = false
    if (username.value !== '' && password.value !== '') {
        axios.post('http://47.120.38.121:5000/api/login', {
            username: username.value,
            password: password.value
        }).then(res => {
            const { data } = res
            if (data?.code === 200) {
                ElMessage({
                    message: '登陆成功',
                    type: 'success',
                })
                token = data.data.token
                showLogin.value = false
            }
            else {
                ElMessage({
                    message: '账号或密码错误',
                    type: 'error',
                })
            }
        }).catch(error => {
            console.log(error)
            ElMessage({
                message: '登录异常',
                type: 'error',
            })

        })
    } else {
        ElMessage({
            message: '请输入账号或密码',
            type: 'warning',
        })
    }


}
</script>
<style scoped>
.login-page {
    display: flex;
    align-items: center;
    justify-content: center;
    width: 100vw;
    height: 100vh;
    background-size: 100% 100%;
    background-size: cover;
    background-repeat: no-repeat;
    background-attachment: fixed;
    /*关键*/
    background-position: center;
    top: 0;
    left: 0;
    background-image: url(../assets/page.jpg);
}

.login-box {
    display: flex;
    flex-direction: column;
   align-items: center;
    width: 350px;
    background-color: rgba(255, 255, 255, .9);
    padding: 30px 50px;
    height: 200px;
    border-radius: 10px;
}
</style>
