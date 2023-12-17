<script setup>
import HelloWorld from './components/HelloWorld.vue'
import TheWelcome from './components/TheWelcome.vue'
</script>

<template>


<head>
    <meta charset="UTF-8">
    <link rel="icon" href="/favicon.ico">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Vite App</title>
    <link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/bootstrap@5.3.2/dist/css/bootstrap.min.css" integrity="sha384-T3c6CoIi6uLrA9TneNEoa7RxnatzjcDSCmG1MXxSR1GAsXEV/Dwwykc2MPK8M2HN" crossorigin="anonymous">
  </head>

  <header>
    <div class="wrapper">
      <HelloWorld msg="Криптографический алгоритм RC2" />
    </div>
  </header>

  <main>
    <TheWelcome />


    <p style="white-space: pre-line;">{{ message }}</p>
    <textarea class="custom-textarea" v-model="message" placeholder="Введите ключ"></textarea>
    <p style="white-space: pre-line;">{{ message }}</p>
    <textarea class="custom-textarea" v-model="message" placeholder="Введите текст для шифрования"></textarea>
    <br>
    <div class = "buttons">
    <button type="button" class="btn btn-dark v-on:click=">Зашифровать</button>
    <button type="button" class="btn btn-dark ">Расшифровать</button>
    </div>
    <p style="white-space: pre-line;">{{ message }}</p>
    <textarea class="custom-textarea" v-model="message" placeholder="Полученный текст"></textarea>
  </main>
</template>




<style scoped>
header {
  line-height: 1.5;
}


@media (min-width: 1024px) {
  header {
    display: flex;
    place-items: center;
    padding-right: calc(var(--section-gap) / 2);
  }
  
  .buttons {
  overflow: hidden; /* Чтобы обертка автоматически расширялась до содержимого */
}

.btn:nth-child(2) {
  margin-right: 308px; /* Отступ справа на 25 пикселей */
  float: right; /* Вторая кнопка отодвигается вправо */
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
