<script setup>
    import { defineProps,defineEmits } from 'vue';

    // props（全称properties）最直白的中文理解是：父组件传给子组件的 “参数 / 属性”
    // 子组件通过 defineProps “声明并校验” 自己要从父组件拿什么数据，相当于给父组件的传参设了 “接收规则”。
    defineProps({//目的是确保子组件接收父组件传入的数据为「数组」
        //下方键值对的值是一个嵌套的对象，作为tosdos这个键对应的 “具体规则内容”；
        todos:{//接收一个叫 todos 的数据
            type: Array,//必须是数组类型
            required: true//且不能为空，type、required为关键字
        }
    })
    // defineProps仅支持两种写法：['参数名']（数组式）、{ 参数名: 规则 }（对象式）；
    // defineProps的返回值是一个「只读的响应式对象」，里面包含了父组件传给子组件的所有 props 数据（比如这里的todos数组）

    const emits=defineEmits(['delete'])//defineEmits(['delete']) 的返回值是一个「事件触发函数，emits即为这个函数
    // defineEmits(['delete']) → 子组件先 “声明”：我能触发一个叫delete的事件；
    // 子组件通过emits('delete', index) → 触发这个delete事件
    // 父组件通过<TodoList @delete="del"> → 监听子组件的delete事件，触发自己的del方法完成删除。    

    function onDelete(index) {
        emits('delete', index); // 触发父组件绑定的 @delete 事件，并传递索引
    }
</script>


<template>
    <!-- v-for的常用数据源数组和对象，如果是数组，第一项 = 数组当前项（必选），第二项 = 索引（可选）
     如果是对象，第一项 = 值（必选），第二项 = 键（可选），第三项 = 索引（可选） -->
    <!-- key是 Vue 给循环渲染的元素加的「唯一身份证」。前后两个index是同一个东西，并且index只是 “当前项索引” 起的「别名」，可以换成任何合法的变量名 -->
    <div v-for="(item,index) in todos" :key="index" :class= "item.isCompleted ? 'item completed' : 'item'">
    <!-- 一个 HTML 元素可以同时拥有多个 class 名，用空格分隔即可；Vue 会把这个规则融入到动态 class 绑定里，最终渲染到页面的元素会同时带上这两个 class 的样式。 -->
        <div>
            <input v-model="item.isCompleted" type="checkbox"/>
            <span class="name">{{item.text}}</span>
        </div>
        <div @click="onDelete(index)" class="del">del</div>
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
</style>