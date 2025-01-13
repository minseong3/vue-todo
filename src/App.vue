<template>
    <TodoHeader></TodoHeader>
    <TodoClock></TodoClock>
    <TodoInput @add="addTodoItem" @clear = "clearAllTodoItems"></TodoInput>
    <TodoList :todoItems = "todoItems" @remove ="removeTodoItem" @complete="completeTodoItem"></TodoList>
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
                const key = localStorage.key(i);
                const todoItem = JSON.parse(localStorage.getItem(key)); // JSON 파싱
                result.push(todoItem);
            }              
            return result;
        }
        todoItems.value = fetchTodos();

        function addTodoItem(todoText) {
            const newTodo = { text: todoText, completed: false }; // 객체 형태로 생성
            todoItems.value.push(newTodo);
            localStorage.setItem(todoText, JSON.stringify(newTodo)); // JSON 형태로 저장
        }

        function completeTodoItem(index) {
            todoItems.value[index].completed = !todoItems.value[index].completed;
            const updatedTodo = todoItems.value[index];
            localStorage.setItem(updatedTodo.text, JSON.stringify(updatedTodo));
        }

        function removeTodoItem(item, index) {
            // 로컬스토리지에서 키를 찾아서 삭제
            const keyToRemove = Object.keys(localStorage).find(key => {
                const storedItem = JSON.parse(localStorage.getItem(key));
                return storedItem.text === item.text && storedItem.completed === item.completed;
            });

            if(keyToRemove) {
                localStorage.removeItem(keyToRemove); // 키로 삭제
            }

            // 메모리에서도 삭제
            todoItems.value.splice(index, 1);
            
        }


        function clearAllTodoItems() {
            localStorage.clear(); // 로컬스토리지 비우기
            todoItems.value = []; // todoItems 초기화
        }


        return {todoItems, addTodoItem, completeTodoItem, removeTodoItem, clearAllTodoItems}
    }
    
}
</script>

<style lang="scss" scoped>

</style>