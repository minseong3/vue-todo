<template>
    <footer class="legend">
      <p>-{{ randomQuote.author }}-<br>
        {{ randomQuote.authorProfile }}<br>
        {{ randomQuote.message }}</p>
    </footer>
  </template>
  
  <script>
  import { ref, onMounted } from 'vue';
  import { kadvice } from 'kadvice';
  
  export default {
    setup() {
      // 명언 배열
      const quotes = kadvice.getAll();
  
      // 랜덤 명언 저장
      const randomQuote = ref('');
  
      // 랜덤 명언 선택 함수
      function selectRandomQuote() {
        const randomIndex = Math.floor(Math.random() * quotes.length);
        randomQuote.value = quotes[randomIndex];
      }
  
      // 컴포넌트가 마운트될 때 랜덤 명언 선택
      onMounted(() => {
        selectRandomQuote();
      });
  
      return { randomQuote };
    }
  };
  </script>
  
  <style scoped>
  @font-face {
    font-family: 'establishRetrosansOTF';
    src: url('https://fastly.jsdelivr.net/gh/projectnoonnu/noonfonts_2112@1.0/establishRetrosansOTF.woff') format('woff');
    font-weight: normal;
    font-style: normal;
  }
  .legend {
  font-family: 'establishRetrosansOTF';
  background-color: #f8f9fa;
  padding: 5px;
  text-align: center;
  font-size: 20px;
  color: #555;
  position: fixed;
  bottom: 0;
  width: 100%;
  border-top: 1px solid #ddd;
  }

  </style>