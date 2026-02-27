<script setup>
import {ref} from "vue";
// Vue 的 ref() 把这个普通数组包了一层，目的是让它变成响应式的 —— 意思是：
// 只要这个数组里的内容变了（比如加一条、删一条、改状态），页面会自动跟着更新，不用你手动改 DOM。
// Vue 只主动监测 “被它包装成响应式” 的数据
import TodoList from "./components/TodoList.vue";

const str=ref("");
// ref()返回的不是原数据，而是一个 “包裹对象”，str实际是{ value: "" }：

const list=ref([
    {text:"吃饭",isCompleted:false},
    {text:"睡觉",isCompleted:true},
    {text:"打代码",isCompleted:false},
]);

function add(){
    const inputValue = str.value.trim();//去除输入框内容的「首尾空格」（比如用户只按了空格，也判定为空）
    if (!inputValue) { // 如果为空则直接返回，不执行后续操作
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
            <!-- Vue 模板的语法规则 ——v-model等指令的等号后，必须用引号包裹 “表达式 / 变量”，否则 Vue 会把它当成普通字符串解析，而非定义的响应式变量。
             Vue 在编译模板时，会自动检测到str是 ref 变量，帮补上.value -->
            <input 
                v-model="str" 
                type="text" 
                class="todo-input" 
                placeholder="Add a todo"
            />
            
            <div @click="add" class="todo-button">Add Todo</div>
        </div>
        <TodoList :todos="list" @delete="del"></TodoList><!-- :告诉 Vue“这是一个动态绑定，右边是变量而不是这里的普通字符串； -->
        
    </div>
</template>

<style scoped>
    .todo-button{
        width: 100px;
        height: 50px;
        background: linear-gradient(to right, rgb(113, 65, 168), rgba(44, 114, 251, 1));/* background-color只认纯色 */
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
        box-sizing: border-box;/*加了之后：50px包含边框+内边距，实际高度就是50px*/
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
