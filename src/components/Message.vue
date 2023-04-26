<template>
    <div class="message-box">
        <el-tabs class="demo-tabs" v-model="type">
            <el-tab-pane label="普通消息" name="0">
                <el-form>
                    <el-form-item label="内容">
                        <el-input :rows="4" type="textarea" placeholder="请输入内容" v-model="content0" />
                    </el-form-item>
                    <div class="btn">
                        <div style="width: 60px;">
                            <el-button type="primary" @click="sendMessage" class="btn">发送</el-button>
                        </div>
                    </div>
                </el-form>

            </el-tab-pane>
            <el-tab-pane label="卡片消息" name="1">
                <el-form>
                    <el-form-item label="标题">
                        <el-input type="primary" placeholder="请输入标题" v-model="title" />
                    </el-form-item>
                    <el-form-item label="内容">
                        <el-input :rows="4" type="textarea" placeholder="请输入内容" v-model="content1" />
                    </el-form-item>
                    <div class="btn">
                        <div style="width: 60px;">
                            <el-button type="primary" @click="sendMessage" class="btn">发送</el-button>
                        </div>
                    </div>

                </el-form>
            </el-tab-pane>
        </el-tabs>
    </div>
</template>

<script setup lang="ts">
import axios from 'axios';
import { defineProps } from 'vue'
import { ref } from 'vue';
import moment from 'moment'

const title = ref('')
const { token } = defineProps({
    token: {
        type: String,
    }
})
const type = ref("0")
const content0 = ref('')
const content1 = ref('')
async function sendMessage() {
    try {
        if (type.value === '0' && content0.value !== '' || type.value === '1' && title.value !== '' && content1.value !== '') {
            let time = new Date()
            let newTime = moment(time).format('YYYY年MM月DD日 HH:mm:ss')
            const params = {
                url: "http://47.120.38.121:5000/api/message",
                method: "POST",
                headers: {
                    Authorization: "Bearer " + token
                },
                data: {
                    type: type.value,
                    title: title.value,
                    content0: content0.value,
                    content1: content1.value,
                    time: newTime
                }
            }
            let { data } = await axios(params)
            if (data.code === 0) {
                ElMessage({
                    message: '发送成功',
                    type: 'success',
                })
                title.value = ''
                content0.value = ''
                content1.value = ''
            } else {
                ElMessage({
                    message: '发送异常',
                    type: 'error',
                })
            }

        } else {
            ElMessage({
                message: '请将消息补充完整',
                type: 'warning',
            })
        }

    } catch (error) {
        ElMessage({
            message: '发送异常',
            type: 'error',
        })
    }
    console.log(token)
    console.log(type.value)
}
// async function sendMessage1() {
//     try {
//         if (type.value === '0' && content0.value !== '' || type.value === '1' && title.value !== '' && content1.value !== '') {
//             const time = new Date()
//             const params = {
//                 url: "http://localhost:8080/api/message",
//                 method: "POST",
//                 headers: {
//                     Authorization: "Bearer " + token
//                 },
//                 data: {
//                     type:type.value,
//                     title:title.value,
//                     content0:content0.value,
//                     content1:content1.value,
//                     time
//                 }
//             }
//             let {data} = await axios(params)
//             if(data.code === 0){
//                 ElMessage({
//                     message: '发送成功',
//                     type: 'success',
//                 })
//             }else{
//                 ElMessage({
//                     message: '发送异常',
//                     type: 'error',
//                 })
//             }

//         } else {
//             ElMessage({
//                 message: '请将消息补充完整',
//                 type: 'warning',
//             })
//         }

//     } catch (error) {
//         ElMessage({
//                 message: '发送异常',
//                 type: 'error',
//             })
//     }
//     console.log(token)
//     console.log(type.value)
// }
</script>
<style scoped>
.message-box {

    width: 500px;
    background-color: rgba(255, 255, 255, .9);
    padding: 30px 50px;
    height: 250px;
    border-radius: 10px;
}

.btn{
    width: 100%;
    display: flex;
    justify-content: center;
}

</style>
