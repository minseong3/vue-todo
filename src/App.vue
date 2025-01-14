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
            todoItems.value.sort((a, b) => a.id - b.id); // todoItems 배열 정렬
        }
        

        function addTodoItem(todoText) {
            const newTodo = { id: Date.now(),text: todoText, completed: false }; // 객체 형태로 생성

            todoItems.value.push(newTodo);
            localStorage.setItem(newTodo.id, JSON.stringify(newTodo)); // 로컬스토리지에 JSON 형태로 저장
        }

        function completeTodoItem(id) {
            const index = todoItems.value.findIndex(item => item.id === id);
            if (index !== -1) {
                todoItems.value[index].completed = !todoItems.value[index].completed;
                localStorage.setItem(id, JSON.stringify(todoItems.value[index])); // ID로 저장
            }
        }

        function removeTodoItem(id, index) {
            localStorage.removeItem(id);
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

<style scoped>
html, body {
  margin: 0; /* 기본 마진 제거 */
  padding: 0; /* 기본 패딩 제거 */
  overflow-x: hidden; /* x축 스크롤 제거 */
  width: 100%; /* 전체 화면에 맞게 설정 */
  box-sizing: border-box; /* 박스 크기 조절 방식 변경 */
}
#app {
    display: grid;
    grid-template-columns: auto auto; /* 내용 크기에 맞게 열 크기 조정 */
    grid-template-rows: auto auto;
}
</style>