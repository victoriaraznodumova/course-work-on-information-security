<script setup>
import HelloWorld from './components/HelloWorld.vue'
import TheWelcome from './components/TheWelcome.vue'
</script>

<template>
  <header>
    <div class="wrapper">
      <HelloWorld msg="Криптографический алгоритм RC2" />
    </div>
  </header>

  <main>
    <TheWelcome />


    
    <p style="white-space: pre-line;">{{ message }}</p>
    <textarea class="custom-textarea" v-model="message" placeholder="Введите текст для шифрования"></textarea>
    <br>
    
  </main>
</template>

<style scoped>
header {
  line-height: 1.5;
}

.logo {
  display: block;
  margin: 0 auto 2rem;
}

@media (min-width: 1024px) {
  header {
    display: flex;
    place-items: center;
    padding-right: calc(var(--section-gap) / 2);
  }

  .logo {
    margin: 0 2rem 0 0;
  }

  .custom-textarea {
    width: 300px;
    height: 200px;
}

  header .wrapper {
    display: flex;
    place-items: flex-start;
    flex-wrap: wrap;
  }
}
</style>
