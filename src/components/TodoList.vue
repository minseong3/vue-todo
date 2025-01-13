<template>
    <ul>
        <p class="t1" v-for="(item, index) in todoItems" :key="index">
            <span :class="{ completed: item.completed }">{{ item.text }}</span>
            <button @click="completeTodo(index)">완료</button>
            <button @click="removeTodo(item, index)">삭제</button>
        </p>
    </ul>
</template>

<script>

    export default {
        props: ['todoItems'],

        setup(props, context) {

            function removeTodo(item, index) {
                context.emit('remove', item, index)
            }

            function completeTodo(index) {
                context.emit('complete', index)
            }

            return { removeTodo, completeTodo }
        }
    }
</script>

<style scoped>
ul {
    font-size: 30px;
    margin-bottom: 5px;
    text-align: center;
}
button {
    background: linear-gradient(to right, #9fd8ff, #ff92aa);
    padding: 10px 15px 10px 15px;
    box-sizing: border-box;
    border-radius: 50px;
    min-width: 70px;
    height: auto;
    font-size: 16px;
    font-weight: 700;
    color: #000000;
    cursor: pointer;
    margin-left: 10px;
    box-shadow: 0px -3px 3px #ffffff, 0px 3px 3px #cccccc;
}
@font-face {
    font-family: 'Ownglyph_StudyHard-Rg';
    src: url('https://fastly.jsdelivr.net/gh/projectnoonnu/2411-3@1.0/Ownglyph_StudyHard-Rg.woff2') format('woff2');
    font-weight: normal;
    font-style: normal;
}
.t1 {font-family: 'Ownglyph_StudyHard-Rg';}
.completed {
  text-decoration: line-through;
  color: gray;
}
</style>