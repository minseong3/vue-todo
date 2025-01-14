<template>
    <TodoHeader></TodoHeader>
    <TodoClock></TodoClock>
    <TodoInput @add="addTodoItem" @clear = "clearAllTodoItems"></TodoInput>
    <TodoList :todoItems = "todoItems" @remove ="removeTodoItem" @complete = "completeTodoItem"></TodoList>
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

        fetchTodos();

        function fetchTodos() {
            todoItems.value = []; //  배열 초기화
            for (let i = 0; i < localStorage.length; i++) {
                const key = localStorage.key(i);
                const todoItem = JSON.parse(localStorage.getItem(key));
                todoItems.value.push(todoItem); // 로컬스토리지에 저장되어 있는 값들을 key 별로 가져와 todoItems 배열에 저장
            }
        }
        

        function addTodoItem(todoText) {
            const newTodo = { text: todoText, completed: false }; // 객체 형태로 생성

            let pk = 0;
            // key 값을 pk로 지정하여 key 값이 null이 아닐 경우 존재하는 key 값이기 때문에 key 값 변경
            while(localStorage.getItem(pk.toString()) !== null) {
                pk++;
            }

            todoItems.value.push(newTodo);
            localStorage.setItem(pk.toString(), JSON.stringify(newTodo)); // 로컬스토리지에 JSON 형태로 저장
        }

        function completeTodoItem(key) {
            const todoItem = JSON.parse(localStorage.getItem(key));

            if(todoItem) {
                todoItem.completed = !todoItem.completed;

                localStorage.setItem(key, JSON.stringify(todoItem));

                const index = todoItems.value.findIndex(item => item.text === todoItem.text);
                if(index !== -1) {
                    todoItems.value[index] = todoItem;
                }
            }
        }

        function removeTodoItem(item, index) {
            const removeKey = Object.keys(localStorage).find(key => {
                const storedItem = JSON.parse(localStorage.getItem(key));
                return storedItem.text === item.text && storedItem.completed === item.completed;
            });

            if(removeKey) {
                localStorage.removeItem(removeKey); //로컬스토리지에서 삭제
            }

            todoItems.value.splice(index, 1); //메모리에서 삭제
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