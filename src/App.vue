<template>
    <TodoHeader></TodoHeader>
    <TodoClock></TodoClock>
    <TodoInput @add="addTodoItem" @clear = "clearAllTodoItems"></TodoInput>
    <TodoList :todoItems = "todoItems" @remove ="removeTodoItem"></TodoList>
    <TodoFooter></TodoFooter>
</template>

<script>
import TodoHeader from '@/components/TodoHeader.vue';
import TodoInput from './components/TodoInput.vue';
import TodoList from './components/TodoList.vue';
import TodoFooter from './components/TodoFooter.vue';
import { ref } from 'vue';
import TodoClock from './components/TodoClock.vue';


export default {
    components: {
        TodoHeader,
        TodoClock,
        TodoInput,
        TodoList,
        TodoFooter
    },
    setup() {
        const todoItems = ref([]);

        function fetchTodos() {
            const result = [];
            for (let i =0; i < localStorage.length; i++) {
                const todoItem = localStorage.key(i);
                result.push(todoItem);
            }              
            return result;
        }
        todoItems.value = fetchTodos();

        function addTodoItem(todo) {
            todoItems.value.push(todo);
            localStorage.setItem(todo, todo);
        }

        function removeTodoItem(item, index) {
            todoItems.value.splice(index, 1);
            localStorage.removeItem(item);
        }


        function clearAllTodoItems() {
            localStorage.clear(); // 로컬스토리지 비우기
            todoItems.value = []; // todoItems 초기화
        }


        return {todoItems, addTodoItem, removeTodoItem, clearAllTodoItems}
    }
    
}
</script>

<style lang="scss" scoped>

</style>