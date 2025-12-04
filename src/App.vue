<script setup>
import {ref} from "vue";

const str=ref("");
const list=ref([
    {text:"吃饭",isCompleted:false},
    {text:"睡觉",isCompleted:true},
    {text:"打代码",isCompleted:false},
]);
function add(){
    const inputValue = str.value.trim();//去除输入框内容的「首尾空格」（比如用户只按了空格，也判定为空）
    if (!inputValue) { // 为空则直接返回，不执行后续操作
        str.value='';// 清空输入框的空格
        return;
    }
    list.value.push({text:str.value,isCompleted:false});
    str.value="";// 清空输入框
}
function del(index){
    list.value.splice(index,1);// 从index开始，删一个元素。数组.splice(起始索引, 删除数量, 要插入的元素1, 要插入的元素2...)
}

</script>

<template>
    <div class="todo-app">
        <div class="title">杨志灿的todo app</div>

        <div class="todo-form">
            <input v-model="str" type="text" class="todo-input" placeholder="add a todo"/><!-- 提示文字 -->
            <div @click="add" class="todo-button">add todo</div>
        </div>

        <div v-for="(item,index) in list" :key="index" :class="item.isCompleted ? 'item completed' : 'item'">
            <div>
                <input v-model="item.isCompleted" type="checkbox"/>
                <span class="name">{{item.text}}</span>
            </div>
            <div @click="del(index)" class="del">del</div>
        </div>
    </div>
</template>

<style scoped>
    .del{
        cursor: pointer;/* 鼠标变手指 */
        user-select: none;/* 禁止选中文字 */
        color: red;
    }
    .completed{
        text-decoration: line-through;
        opacity: 0.4;
        user-select: none;/* 禁止选中文字 */
    }
    .item{
        display:flex;/* 弹性盒子模型 */
        align-items: center;/* 水平居中 */
        justify-content: space-between;/* 左右两端对齐 */
        width: 80%;
        height: 50px;
        margin: 15px auto;/* 上下外边距15px，左右自动，由于上面的盒子margin-bottom: 20px，只有大于20px第一个盒子才会下移 */
        padding: 16px;
        box-sizing: border-box;
        border-radius: 20px;
        box-shadow: rgba(149, 157, 165, 0.2) 0px 8px 20px;
    }
    .todo-button{
        width: 100px;
        height: 50px;
        background: linear-gradient(
            to right, rgb(113, 65, 168), rgba(44, 114, 251, 1));/* background-color只认纯色 */
        color: white;/* 文字颜色 */
        border-radius: 0 20px 20px 0;/* 左上开始顺时针转一圈 */
        display: flex;/* 变成弹性盒子模型,子元素默认「横向排列」 */
        justify-content: center;/* 水平居中 */
        align-items: center;/* 垂直居中 */
        cursor: pointer;/* 鼠标变手指 */
        user-select: none;/* 禁止选中文字 */
        /*<button>、<input>才有边框*/
    }
    .todo-input{
        border: 1px solid #dfe1e5;/* 边框颜色 */
        outline: none;/* 去掉输入框选中时的边框 */
        width: 60%;
        height: 50px;
        border-radius: 20px 0 0 20px;/* 左上开始顺时针转一圈 */
        font-size: 16px;
        padding-left: 15px;
        box-sizing: border-box;/*加了之后：50px包含边框+内边距，实际高度就是52px*/
    }
    .todo-form{
        display: flex;/* 横向排列 */
        padding-left: 30px;
        padding-top: 20px;
        align-items: center;/* 在flex背景下，输入框和按钮垂直对齐 */
        margin-bottom: 20px;
    }
    .title{
        text-align: center;
        font-size: 30px;
        font-weight: bold;
    }
    .todo-app{
        width: 98%;
        height: 500px;
        background-color: white;
        border-radius: 5px;/* 圆角 */
        margin-left: 1%;
        margin-top: 40px;
        padding-top: 30px;
        box-sizing: border-box;/* 包括padding和border在内计算宽高 */
    }
    
</style>
