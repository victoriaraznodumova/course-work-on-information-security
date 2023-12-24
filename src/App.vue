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
    <textarea id="key-textarea" rows="7" cols="38" type="text" v-model="key" placeholder="Введите ключ"></textarea>
    <p style="white-space: pre-line;"></p>
    <textarea id="encrypt-textarea" rows="7" cols="38" v-model="message" placeholder="Введите текст для шифрования"></textarea>
    <br>
    <div class = "buttons">
      <button v-on:click="displayEncryption(Encryption())" type="button" class="btn btn-dark">Зашифровать</button>
      <button v-on:click="displayDecryption(keyExpantion3(key))" type="button" class="btn btn-dark ">Расшифровать</button>
    </div>
    <p style="white-space: pre-line;"></p>
    <textarea id="decrypt-textarea" rows="7" cols="38" v-model="result" placeholder="Полученный текст"></textarea>
  </main>
</template>


<script>
export default {
    data() {
        return {
            
            input: "",
            key: "",
            output: "",
            roundKeyCount: 64,
            expandedKey: [],

        }
    },
    

    methods: {
        
      keyExpantion2 (key){
        return [21, 5, 25, 223, 54, 140, 230,  19];
      },

      
      keyExpantion3 (key){
        return [15, 45, 64, 226, 36, 164, 155, 5];
      },



      keyExpantion(key){
          // Функция для выполнения циклического сдвига влево на байте
          function leftShift(key, shift) {
            return (key << shift) | (key >>> (32 - shift));
          }

          // Функция для применения S-Box замен
          function applySBox(key) {
            key = key.toString(16);
            while (key.length < 8) {
              key = '0' + key;
            }
            
            const sBox = [0xd9, 0x78, 0xf9, 0xc4, 0x19, 0xdd, 0xb5, 0xed, 0x28, 0xe9, 0xfd, 0x79, 0x4a, 0xa0, 0xd8, 
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
            0xc5, 0xf3, 0xdb, 0x47, 0xe5, 0xa5, 0x9c, 0x77, 0x0a, 0xa6, 0x20, 0x68, 0xfe, 0x7f, 0xc1, 0xad];

            let result = '';
            for (let i = 0; i < 8; i += 2) {
              const row = parseInt(key[i], 16);
              const col = parseInt(key[i + 1], 16);
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

                let prevWord = expandedKey[i - 4];
                expandedKey.push(prevWord ^ temp);
              } else {
                let prevWord = expandedKey[i - 4];
                let temp = expandedKey[i - 1];
                expandedKey.push(prevWord ^ temp);
              }
            }

            return expandedKey;
          }

            // Пример использования
            const initialKey = [0x2b, 0x7e, 0x15, 0x16, 0x77, 0xbd, 0x8b, 0x99]; // Ключ длиной 128 бит (16 байт)
            const expandedKey = expandKey(initialKey);




            //expandedKey = [21, 5, 25, 223, 54, 140, 230,  19];
            return expandedKey; 

            //return expandedKey.map(word => word);
            
        },

        Key(){
            return [43,126,21,22,452984875,452984917,452984896,452984918,43,452984958,62,452984936,452984875,85,107,452984835,43,126,21,452984854,452984875,452984917,452984896,86,43,452984958,62,104,452984875,85,107,3,43,126,21,22,452984875,452984917,452984896,452984918,43,452984958,62,452984936,452984875,85,107,452984835,43,126,21,452984854,452984875,452984917,452984896,86,43,452984958,62,104,452984875,85,107,3]
          },



        Encryption(){
          function mix_encryption(R, key){
            const result = [];
            for (let i = 0; i<4; i++){
              const f = (key[i] ^ (R[(i-1) % 4] && R[(i-2) % 4]) ^ ((R[(i-1) % 4] ^ R[(i-1) % 4]) && R[(i-3) % 4])) & 0xFFFF;
              const value = (R[i] ^ f) & 0xFFFF;
              result.push(value)
            }
            return result;
          }

          function mash_encryption (R, key){
            const result = [];
              for (let i = 0; i<4; i++){
                  let n = R[(i-1) % 4] && 63;
                  let value = (R[i] ^ key[n]) & 0xFFFF;
                  result.push(value)
              }
              return result;
          }

          const R = [0x15, 0x2d, 0x64, 0xe2, 0x36, 0xa4, 0x9b, 0x05];

  //15, 45, 64, 226, 36, 164, 155, 5

  //0x2b, 0x7e, 0x15, 0x16, 0x77, 0xbd, 0x8b, 0x99
  //43, 126, 15, 16, 77, 189, 139, 99

          const R_14 = R.slice(0, 4);
          const R_58 = R.slice(4, 8);

          let enc_key = this.keyExpantion();
          const sliced_key_encryption = [];

          for (let i = 0; i < enc_key.length; i += 4) {
            sliced_key_encryption.push(enc_key.slice(i, i + 4));
          }


          const R_14_encryption_1 = mix_encryption(R_14, sliced_key_encryption[0]);
          //console.log ("mix 1st round", R_14_encryption_1);
          const R_58_encryption_1 = mix_encryption(R_58, sliced_key_encryption[1]);
          //console.log ("mix 2nd round", R_58_encryption_1);
          const R_14_encryption_2 = mix_encryption(R_14_encryption_1, sliced_key_encryption[2]);
          //console.log ("mix 3th round", R_14_encryption_2);
          const R_58_encryption_2 = mix_encryption(R_58_encryption_1, sliced_key_encryption[3]);
          //console.log ("mix 4th round", R_58_encryption_2);
          const R_14_encryption_3 = mix_encryption(R_14_encryption_2, sliced_key_encryption[4]);
          //console.log ("mix 5th round", R_14_encryption_3);


          const R_58_encryption_3 = mash_encryption(R_58_encryption_2, enc_key);
          //console.log ("mash 1st round", R_58_encryption_3);


          const R_14_encryption_4 = mix_encryption(R_14_encryption_3, sliced_key_encryption[5]);
          //console.log ("mix 6th round", R_14_encryption_4);
          const R_58_encryption_4 = mix_encryption(R_58_encryption_3, sliced_key_encryption[6]);
          //console.log ("mix 7th round", R_58_encryption_4);
          const R_14_encryption_5 = mix_encryption(R_14_encryption_4, sliced_key_encryption[7]);
          //console.log ("mix 8th round", R_14_encryption_5);
          const R_58_encryption_5 = mix_encryption(R_58_encryption_4, sliced_key_encryption[8]);
          //console.log ("mix 9th round", R_58_encryption_5);
          const R_14_encryption_6 = mix_encryption(R_14_encryption_5, sliced_key_encryption[9]);
          //console.log ("mix 10th round", R_14_encryption_6);
          const R_58_encryption_6 = mix_encryption(R_58_encryption_5, sliced_key_encryption[10]);
          //console.log ("mix 11th round", R_58_encryption_6);


          const R_14_encryption_7 = mash_encryption(R_14_encryption_6, enc_key);
          //console.log ("mash 2nd round", R_14_encryption_7);


          const R_58_encryption_7 = mix_encryption(R_58_encryption_6, sliced_key_encryption[11]);
          //console.log ("mix 12th round", R_58_encryption_7);
          const R_14_encryption_8 = mix_encryption(R_14_encryption_7, sliced_key_encryption[12]);
          //console.log ("mix 13th round", R_14_encryption_8);
          const R_58_encryption_8 = mix_encryption(R_58_encryption_7, sliced_key_encryption[13]);
          //console.log ("mix 14th round", R_58_encryption_8);
          const R_14_encryption = mix_encryption(R_14_encryption_8, sliced_key_encryption[14]);
          //console.log ("mix 15th round", R_14_encryption);
          const R_58_encryption = mix_encryption(R_58_encryption_8, sliced_key_encryption[15]);
          //console.log ("mix 16th round", R_58_encryption);

          

          const encryption = R_14_encryption.concat(R_58_encryption);
          const hexArray = encryption.map(item => item.toString(16));
          return hexArray;

          },



          




          



      
      displayEncryption(key) {
          this.encryptedText = key;
          document.getElementById("decrypt-textarea").value = this.encryptedText;
        },


        displayDecryption(key) {
          this.decryptedText = key;
          document.getElementById("decrypt-textarea").value = this.decryptedText;          
        } 
      },



        encryption(input, key){
          input;

        },



        decryption(output, key){

        },

        

        

// Пример использования
// const initialKey = [0x2b, 0x7e, 0x15, 0x16, 0x77, 0xbd, 0x8b, 0x99]; // Ключ длиной 128 бит (16 байт)
// const expandedKey = expandKey(initialKey);

// console.log('Расширенный ключ:');
// console.log(expandedKey.map(word => word));

          


//10 3A CG 64 0D 51 1E 89


       


        innerFunction(){

        },





      
          

    }





























//////////////////////////////////////













// const encryption_14 = encryption.slice(0, 4);
// const encryption_58 = encryption.slice(4, 8);











// ////// key preparation
// key_decryption = sliced_key_encryption.reverse();
// const sliced_key_decryption = key_decryption.map(innerArray => innerArray.reverse());
// console.log (sliced_key_decryption[0]);
// ////// key preparation





// function mix_decryption (R, key){
//     let result = [];
    
//     for (let i = 0; i<4; i++){
//        let S = 0; 
//         if ( i == 0){
//             S = 1;
//         }
//         else if ( i == 1){
//             S = 2;
//         }
//         else if ( i == 2){
//             S = 3;
//         }
    
//         else if ( i == 3){
//             S = 5;
//         }
        
//         let change = 0;
//         for (let j = 0; j < R[i].length; j++) {
//             R[j] = (R[j] >> S) || ((R[j] << (8 - S)) & 255);
//             change = R[j]
//         }
        
//         R[i]=change
       
//         let value = (R[i] - key[i] - (R[(i-1) % 4] && R[(i-2) % 4]) - ((R[(i-1) % 4] ^ R[(i-1) % 4]) && R[(i-3) % 4])) & 0xFFFF;
        
//         result.push(value)
        
        
//   }
    
    
//     return result;
// }


//         for (let i = 0; i<4; i++){
//        let S = 0; 
//         if ( i == 0){
//             S = 1;
//         }
//         else if ( i == 1){
//             S = 2;
//         }
//         else if ( i == 2){
//             S = 3;
//         }
    
//         else if ( i == 3){
//             S = 5;
//         }
        
//         let Rt = [21, 5, 25, 223]
//         for (let j = 0; j < Rt.length; j++) {
//             Rt[j] = (Rt[j] >> S) || ((Rt[j] << (8 - S)) & 255);
//             console.log ("142", Rt[j], " ", j)
//         }
//         }




// let R_14_decryption_1 = mix_decryption(encryption_14, sliced_key_decryption[0]);
// console.log("1", R_14_decryption_1);

// let R_58_decryption_1 = mix_decryption(encryption_58, sliced_key_decryption[1]);
// console.log("2", R_58_decryption_1);

// let R_14_decryption_2 = mix_decryption(R_14_decryption_1, sliced_key_decryption[2]);
// console.log("3", R_14_decryption_2);

// let R_58_decryption_2 = mix_decryption(R_58_decryption_1, sliced_key_decryption[3]);
// console.log("4", R_58_decryption_2);






// console.log("decryption");





// function mix_decryption (R, key){
//     let result = [];
    
//     for (let i = 0; i<4; i++){
//        let S = 0; 
//         if ( i == 0){
//             S = 1;
//         }
//         else if ( i == 1){
//             S = 2;
//         }
//         else if ( i == 2){
//             S = 3;
//         }
    
//         else if ( i == 3){
//             S = 5;
//         }
        
//         let num = R[i];
//         num = (num >> S) || ((num << (8 - S)) & 255);
        
        
        
//         let value = (num - key[i] - (R[(i-1) % 4] && R[(i-2) % 4]) - ((R[(i-1) % 4] ^ R[(i-1) % 4]) && R[(i-3) % 4])) & 0xFFFF;
        
//         result.push(value)
        
//   }
//     return result;
// }

// function mash_decryption (R, key){
    
//     let result = [];
//     for (let i = 0; i<4; i++){
//         let n = R[(i-1) % 4] && 63;
//         value = (R[i] - key[n]) & 0xFFFF;
//         result.push(value)
//     }
//     return result;
// }
        





//     console.log("decryption");
    
//     let R_14_decryption_1 = mix_decryption(encryption_14, sliced_key_decryption[0]);
//     console.log ("mix 1st round", R_14_decryption_1)
    
//     let R_58_decryption_1 = mix_decryption(encryption_58, sliced_key_decryption[1]);
//     console.log("mix 2nd round", R_58_decryption_1);
    
//     let R_14_decryption_2 = mix_decryption(R_14_decryption_1, sliced_key_decryption[2]);
//     console.log("mix 3th round", R_14_decryption_2);
    
//     let R_58_decryption_2 = mix_decryption(R_58_decryption_1, sliced_key_decryption[3]);
//     console.log("mix 4th round", R_58_decryption_2);
    
    
//     let R_14_decryption_3 = mix_decryption(R_14_decryption_2, sliced_key_decryption[4]);
//     console.log("mix 5th round", R_14_decryption_3);
    
    
    
//     let R_58_decryption_3 = mash_decryption(R_58_decryption_2, key_rev);
//     console.log("mash 1th round", R_58_decryption_3);
    
    

    
//     let R_14_decryption_4 = mix_decryption(R_14_decryption_3, sliced_key_decryption[5]);
//     console.log("mix 7th round", R_14_decryption_4);
    
//     let R_58_decryption_4 = mix_decryption(R_58_decryption_3, sliced_key_decryption[6]);
//     console.log("mix 8th round", R_58_decryption_4);
    
//     let R_14_decryption_5 = mix_decryption(R_14_decryption_4, sliced_key_decryption[7]);
//     console.log("mix 9th round", R_14_decryption_5);
    
//     let R_58_decryption_5 = mix_decryption(R_58_decryption_4, sliced_key_decryption[8]);
//     console.log("mix 10th round", R_58_decryption_5);
    
//     let R_14_decryption_6 = mix_decryption(R_14_decryption_5, sliced_key_decryption[9]);
//     console.log("mix 11th round", R_14_decryption_6);
    
//     let R_58_decryption_6 = mix_decryption(R_58_decryption_5, sliced_key_decryption[10]);
//     console.log("mix 12th round", R_58_decryption_6);
    
    
    
    
    
//     let R_14_decryption_7 = mash_decryption(R_14_decryption_6, key_rev);
//         console.log("mash 2nd round", R_58_decryption_6);
    
    
    
    
    
    
//     let R_58_decryption_7 = mix_decryption(R_58_decryption_6, sliced_key_decryption[11]);
//     console.log("mix 13th round", R_58_decryption_7);
    
//     let R_14_decryption_8 = mix_decryption(R_14_decryption_7, sliced_key_decryption[12]);
//     console.log("mix 14th round", R_14_decryption_8);
    
//     let R_58_decryption_8 = mix_decryption(R_58_decryption_7, sliced_key_decryption[14]);
//     console.log("mix 15th round", R_58_decryption_8);
    
//     let R_14_decryption_9 = mix_decryption(R_14_decryption_8, sliced_key_decryption[15]);
//     console.log("mix 16th round", R_14_decryption_9);
    
    















































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







