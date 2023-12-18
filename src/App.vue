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


    <p style="white-space: pre-line;"></p>
    <textarea id="key-textarea" rows="7" cols="38" v-model="key" placeholder="Введите ключ"></textarea>
    <p style="white-space: pre-line;">{{ message }}</p>
    <textarea id="encrypt-textarea" rows="7" cols="38" v-model="message" placeholder="Введите текст для шифрования"></textarea>
    <br>
    <div class = "buttons">
      <button v-on:click="displayText(key)" type="button" class="btn btn-dark">Зашифровать</button>
    <button type="button" class="btn btn-dark ">Расшифровать</button>
    </div>
    <p style="white-space: pre-line;">{{ message }}</p>
    <textarea id="decrypt-textarea" rows="7" cols="38" v-model="message" placeholder="Полученный текст"></textarea>
  </main>
</template>


<script>
export default {
    data() {
        return {
            exampleGraph: [
                [0, 6, 2, 10, 0],
                [0, 0, 1, 0, 6],
                [0, 0, 0, 9, 3],
                [0, 0, 0, 0, 7],
                [0, 0, 0, 0, 0],
            ],
            source: 0,
            sink: 0,
            maxFlow: -1,
            nodesNumberInput: 2,
            nodesNumber: 0,
            matrixInput: [[0, 0], [0, 0]],
            matrix: null,
            paths: [],
            pathsOutput: [],
            isReadyForGraph: false,




            input: "",
            key: "",
            output: "",
            roundKeyCount: 64,
            sBox: [0xd9, 0x78, 0xf9, 0xc4, 0x19, 0xdd, 0xb5, 0xed, 0x28, 0xe9, 0xfd, 0x79, 0x4a, 0xa0, 0xd8, 
            0x9d, 0xc6, 0x7e, 0x37, 0x83, 0x2b, 0x76, 0x53, 0x8e, 0x62, 0x4e, 0x64, 0x88, 0x44, 0x8b, 0xfb, 0xa2, 
            0x17, 0x9a, 0x59, 0xf5, 0x87, 0xb3, 0x4f, 0x13, 0x61, 0x45, 0x6d, 0x8d, 0x09, 0x81, 0x7d, 0x32, 
            0xbd, 0x8f, 0x40, 0xeb, 0x86, 0xb7, 0x7b, 0x0b, 0xf0, 0x95, 0x21, 0x22, 0x5c, 0x6b, 0x4e, 0x82, 
            0x54, 0xd6, 0x65, 0x93, 0xce, 0x60, 0xb2, 0x1c, 0x73, 0x56, 0xc0, 0x14, 0xa7, 0x8c, 0xf1, 0xdc, 
            0x12, 0x75, 0xca, 0x1f, 0x3b, 0xbe, 0xe4, 0xd1, 0x42, 0x3d, 0xd4, 0x30, 0xa3, 0x3c, 0xb6, 0x26, 
            0x6f, 0xbf, 0x0e, 0xda, 0x46, 0x69, 0x07, 0x57, 0x27, 0xf2, 0x1d, 0x9b, 0xbc, 0x94, 0x43, 0x03, 
            0xf8, 0x11, 0xc7, 0xf6, 0x90, 0xef, 0x3e, 0xe7, 0x06, 0xc3, 0xd5, 0x2f, 0xc8, 0x66, 0x1e, 0xd7, 
            0x08, 0xe8, 0xea, 0xde, 0x80, 0x52, 0xee, 0xf7, 0x84, 0xaa, 0x72, 0xac, 0x35, 0x4d, 0x6a, 0x2a, 
            0x96, 0x1a, 0xd2, 0x71, 0x5a, 0x15, 0x49, 0x74, 0x4b, 0x9f, 0xd0, 0x5e, 0x04, 0x18, 0xa4, 0xec, 
            0xc2, 0xe0, 0x41, 0x6e, 0x0f, 0x51, 0xcb, 0xcc, 0x24, 0x91, 0xaf, 0x50, 0xa1, 0xf4, 0x70, 0x39, 
            0x99, 0x7c, 0x3a, 0x85, 0x23, 0xb8, 0xb4, 0x7a, 0xfc, 0x02, 0x36, 0x5b, 0x25, 0x55, 0x97, 0x31, 
            0x2d, 0x5d, 0xfa, 0x98, 0xe3, 0x8a, 0x92, 0xae, 0x05, 0xdf, 0x29, 0x10, 0x67, 0x6c, 0xba, 0xc9, 
            0xd3, 0x00, 0xe6, 0xcf, 0xe1, 0x9e, 0xa8, 0x2c, 0x63, 0x16, 0x01, 0x3f, 0x58, 0xe2, 0x89, 0xa9, 
            0x0d, 0x38, 0x34, 0x1b, 0xab, 0x33, 0xff, 0xb0, 0xbb, 0x48, 0x0c, 0x5f, 0xb9, 0xb1, 0xcd, 0x2e, 
            0xc5, 0xf3, 0xdb, 0x47, 0xe5, 0xa5, 0x9c, 0x77, 0x0a, 0xa6, 0x20, 0x68, 0xfe, 0x7f, 0xc1, 0xad],
            expandedKey: [],

        }
    },

    methods: {
        displayText(key) {
          var text = "Пример текста";  // Здесь может быть ваш текст
          document.getElementById("decrypt-textarea").value = key;
        },

        encryption(input, key){
          input;

        },



        decryption(output, key){

        },



        keyExpantion(key){
          function leftShift(key, shift) {
            return (key << shift) | (key >>> (32 - shift));
          }

          function applySBox(key) {
            key = key.toString(16);
            while (key.length < 8) {
              key = '0' + key;
            }

            for (let i = 0; i < 8; i += 2) {
              const row = parseInt(key[i], 16);
              const col = parseInt(key[i + 1], 16);
              result += sBox[row][col];
            }
            return parseInt(result, 16);
          }

          function expandKey(initialKey) {
            for (let i = 0; i < roundKeyCount; i++) {
              if (i < 4) {
                expandedKey.push(initialKey[i]); // Первые 4 слова - это начальный ключ
              } else if (i % 4 === 0) {
                let temp = expandedKey[i - 1];

                if (i % 4 === 0) {
                  temp = applySBox(leftShift(temp, 8)) ^ (0x1B << 24);
                }

                const prevWord = expandedKey[i - 4];
                expandedKey.push(prevWord ^ temp);
              } else {
                const prevWord = expandedKey[i - 4];
                const temp = expandedKey[i - 1];
                expandedKey.push(prevWord ^ temp);
              }
            }
          }
          return expandedKey; 
        },

// Пример использования
// const initialKey = [0x2b, 0x7e, 0x15, 0x16, 0x77, 0xbd, 0x8b, 0x99]; // Ключ длиной 128 бит (16 байт)
// const expandedKey = expandKey(initialKey);

// console.log('Расширенный ключ:');
// console.log(expandedKey.map(word => word));

          





       


        innerFunction(){

        },






          

    }
}














/*код для расширения ключа
// Функция для выполнения циклического сдвига влево на байте
function leftShift(word, shift) {
  return (word << shift) | (word >>> (32 - shift));
}

// Функция для применения S-Box замен
function applySBox(word) {
  word = word.toString(16);
  while (word.length < 8) {
    word = '0' + word;
  }
  
  const sBox = [0xd9, 0x78, 0xf9, 0xc4, 0x19, 0xdd, 0xb5, 0xed, 0x28, 0xe9, 0xfd, 0x79, 0x4a, 0xa0, 0xd8, 0x9d, 0xc6, 0x7e, 0x37, 0x83, 0x2b, 0x76, 0x53, 0x8e, 0x62, 0x4e, 0x64, 0x88, 0x44, 0x8b, 0xfb, 0xa2, 0x17, 0x9a, 0x59, 0xf5, 0x87, 0xb3, 0x4f, 0x13, 0x61, 0x45, 0x6d, 0x8d, 0x09, 0x81, 0x7d, 0x32, 0xbd, 0x8f, 0x40, 0xeb, 0x86, 0xb7, 0x7b, 0x0b, 0xf0, 0x95, 0x21, 0x22, 0x5c, 0x6b, 0x4e, 0x82, 0x54, 0xd6, 0x65, 0x93, 0xce, 0x60, 0xb2, 0x1c, 0x73, 0x56, 0xc0, 0x14, 0xa7, 0x8c, 0xf1, 0xdc, 0x12, 0x75, 0xca, 0x1f, 0x3b, 0xbe, 0xe4, 0xd1, 0x42, 0x3d, 0xd4, 0x30, 0xa3, 0x3c, 0xb6, 0x26, 0x6f, 0xbf, 0x0e, 0xda, 0x46, 0x69, 0x07, 0x57, 0x27, 0xf2, 0x1d, 0x9b, 0xbc, 0x94, 0x43, 0x03, 0xf8, 0x11, 0xc7, 0xf6, 0x90, 0xef, 0x3e, 0xe7, 0x06, 0xc3, 0xd5, 0x2f, 0xc8, 0x66, 0x1e, 0xd7, 0x08, 0xe8, 0xea, 0xde, 0x80, 0x52, 0xee, 0xf7, 0x84, 0xaa, 0x72, 0xac, 0x35, 0x4d, 0x6a, 0x2a, 0x96, 0x1a, 0xd2, 0x71, 0x5a, 0x15, 0x49, 0x74, 0x4b, 0x9f, 0xd0, 0x5e, 0x04, 0x18, 0xa4, 0xec, 0xc2, 0xe0, 0x41, 0x6e, 0x0f, 0x51, 0xcb, 0xcc, 0x24, 0x91, 0xaf, 0x50, 0xa1, 0xf4, 0x70, 0x39, 0x99, 0x7c, 0x3a, 0x85, 0x23, 0xb8, 0xb4, 0x7a, 0xfc, 0x02, 0x36, 0x5b, 0x25, 0x55, 0x97, 0x31, 0x2d, 0x5d, 0xfa, 0x98, 0xe3, 0x8a, 0x92, 0xae, 0x05, 0xdf, 0x29, 0x10, 0x67, 0x6c, 0xba, 0xc9, 0xd3, 0x00, 0xe6, 0xcf, 0xe1, 0x9e, 0xa8, 0x2c, 0x63, 0x16, 0x01, 0x3f, 0x58, 0xe2, 0x89, 0xa9, 0x0d, 0x38, 0x34, 0x1b, 0xab, 0x33, 0xff, 0xb0, 0xbb, 0x48, 0x0c, 0x5f, 0xb9, 0xb1, 0xcd, 0x2e, 0xc5, 0xf3, 0xdb, 0x47, 0xe5, 0xa5, 0x9c, 0x77, 0x0a, 0xa6, 0x20, 0x68, 0xfe, 0x7f, 0xc1, 0xad
  ];

  let result = '';
  for (let i = 0; i < 8; i += 2) {
    const row = parseInt(word[i], 16);
    const col = parseInt(word[i + 1], 16);
    result += sBox[row][col];
  }

  return parseInt(result, 16);
}

// Функция для расширения ключа
function expandKey(initialKey) {
  const roundKeyCount = 64; // Количество раундовых ключей (16*4)

  const expandedKey = [];
  for (let i = 0; i < roundKeyCount; i++) {
    if (i < 4) {
      expandedKey.push(initialKey[i]); // Первые 4 слова - это начальный ключ
    } else if (i % 4 === 0) {
      let temp = expandedKey[i - 1];

      if (i % 4 === 0) {
        temp = applySBox(leftShift(temp, 8)) ^ (0x1B << 24);
      }

      const prevWord = expandedKey[i - 4];
      expandedKey.push(prevWord ^ temp);
    } else {
      const prevWord = expandedKey[i - 4];
      const temp = expandedKey[i - 1];
      expandedKey.push(prevWord ^ temp);
    }
  }

  return expandedKey;
}

// Пример использования
const initialKey = [0x2b, 0x7e, 0x15, 0x16, 0x77, 0xbd, 0x8b, 0x99]; // Ключ длиной 128 бит (16 байт)
const expandedKey = expandKey(initialKey);

console.log('Расширенный ключ:');
console.log(expandedKey.map(word => word));
*/ 







/*
function rc2Encrypt(message, key) {
  // Необходимые константы RC2
  var p = [], c = [], k = [];
  var i, j, t, u;

  // Инициализация ключа
  for (i = 0; i < 256; i++) {
    k[i] = key.charCodeAt(i % key.length);
    p[i] = i;
  }

  // Перемешивание ключа
  for (i = j = 0; i < 256; i++) {
    j = (j + p[i] + k[i]) % 256;
    t = p[i];
    p[i] = p[j];
    p[j] = t;
  }

  // Шифрование сообщения
  i = j = 0;
  var cipher = "";
  for (var idx = 0; idx < message.length; idx++) {
    i = (i + 1) % 256;
    j = (j + p[i]) % 256;
    t = (p[i] + p[j]) % 256;
    u = p[t];
    cipher += String.fromCharCode(message.charCodeAt(idx) ^ u);
  }

  return cipher;
}

function rc2Decrypt(cipher, key) {
  return rc2Encrypt(cipher, key); // RC2 - симметричный алгоритм, для расшифровки нужно передать тот же ключ
}

// Пример использования
var plaintext = "Hello, world!";
var key = "MySecretKey";

var encrypted = rc2Encrypt(plaintext, key);
var decrypted = rc2Decrypt(encrypted, key);

console.log("Шифрованное сообщение:", encrypted);
console.log("Расшифрованное сообщение:", decrypted);
*/

</script>











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
  margin-right: 306px; /* Отступ справа на 25 пикселей */
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







