<script setup>
    import { defineProps,defineEmits } from 'vue';

    defineProps({//目的是确保子组件接收父组件传入的数据为「数组」
        todos:{//接收一个叫 todos 的数据
            type: Array,//(键值对)必须是数组类型
            required: true//且不能为空，type、required为关键字
        }
    })
    const emits=defineEmits(['delete'])    
    function onDelete(index) {
        emits('delete', index); // 触发父组件绑定的 @delete 事件，并传递索引
    }
</script>


<template>
    <div v-for="(item,index) in todos" :key="index" :class="item.isCompleted ? 'item completed' : 'item'">
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