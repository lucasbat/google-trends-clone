<template>
  <div class="card-grid" v-if="texts.length > 0">
    <!-- <Transition name="slide-up"> -->
    <!-- Renderiza os cards em uma grade de 5x5 -->
    <!-- <div
        v-for="(text, index) in 25"
        :key="index"
        class="card"
        :style="{
          backgroundColor: backgroundColors[index],
          transition: transitionStyles[index],
        }"
      > -->
    <div
      v-for="(text, index) in 25"
      :key="index"
      class="card"
      :style="{
        backgroundColor: backgroundColors[index],
        transition: transitionStyles[index],
      }"
    >
      <Transition :name="randomTransitionDirection()">
        <div
          v-if="digitando[index]"
          
        >
     
          <div
            :href="
              'https://www.google.com/search?q=' +
              textoParaLink('ignora', 'ignora')
            "
            target="_blank"
            class="displayed-text"
          >
            {{ displayedTexts[index] }}
          </div>
        </div>

        <!-- <div v-else-if="docState === 'edited'">
          <div
            :href="
              'https://www.google.com/search?q=' +
              textoParaLink('ignora', 'ignora')
            "
            target="_blank"
            class="displayed-text"
          >
            {{ displayedTexts[index] }} 22 {{ digitando[index] }}
          </div>
        </div> -->
      </Transition>
    </div>
    <!-- <a
          :href="
            'https://www.google.com/search?q=' + textoParaLink('ignora', 'ignora')
          "
          target="_blank"
          class="displayed-text"
        >
          {{ displayedTexts[index] }} {{ text }}
        </a> -->

    <!-- </div> -->
    <!-- </Transition> -->
  </div>
  <span class="text-country"
    >Showing the latest hot searches in
    <select v-model="selectedCountry" @change="fetchData(true)">
      <option
        v-for="country in countryNames"
        :key="country.key"
        :value="country.key"
      >
        {{ country.value }}
      </option>
    </select>
  </span>
</template>

<script setup>
import { ref, reactive, Transition, onBeforeMount } from "vue";
import axios from "axios";
const docState = ref("saved");

// Cores de background
let colors = reactive([
  "rgb(52, 168, 82)", // Verde
  "rgb(234, 67, 53)", // Vermelho
  "rgb(250, 187, 5)", // Amarelo
  "rgb(66, 133, 244)", // Azul
]);
const selectedCountry = ref("");

let countryNames = reactive([
  { key: "united_states", value: "United States" },
  { key: "india", value: "India" },
  { key: "japan", value: "Japan" },
  { key: "singapore", value: "Singapore" },
  { key: "israel", value: "Israel" },
  { key: "australia", value: "Australia" },
  { key: "hong_kong", value: "Hong Kong" },
  { key: "taiwan", value: "Taiwan" },
  { key: "canada", value: "Canada" },
  { key: "germany", value: "Germany" },
  { key: "netherlands", value: "Netherlands" },
  { key: "indonesia", value: "Indonesia" },
  { key: "south_korea", value: "South Korea" },
  { key: "turkey", value: "Turkey" },
  { key: "philippines", value: "Philippines" },
  { key: "italy", value: "Italy" },
  { key: "vietnam", value: "Vietnam" },
  { key: "egypt", value: "Egypt" },
  { key: "argentina", value: "Argentina" },
  { key: "poland", value: "Poland" },
  { key: "colombia", value: "Colombia" },
  { key: "ukraine", value: "Ukraine" },
  { key: "saudi_arabia", value: "Saudi Arabia" },
  { key: "kenya", value: "Kenya" },
  { key: "chile", value: "Chile" },
  { key: "romania", value: "Romania" },
  { key: "south_africa", value: "South Africa" },
  { key: "belgium", value: "Belgium" },
  { key: "sweden", value: "Sweden" },
  { key: "austria", value: "Austria" },
  { key: "switzerland", value: "Switzerland" },
  { key: "greece", value: "Greece" },
  { key: "denmark", value: "Denmark" },
  { key: "norway", value: "Norway" },
  { key: "nigeria", value: "Nigeria" },
  { key: "new zealand", value: "New Zealand" },
  { key: "ireland", value: "Ireland" },
  { key: "czech_republic", value: "Czech Republic" },
  { key: "portugal", value: "Portugal" },
  { key: "mexico", value: "Mexico" },
  { key: "malaysia", value: "Malaysia" },
  { key: "hungary", value: "Hungary" },
  { key: "russia", value: "Russia" },
  { key: "thailand", value: "Thailand" },
  { key: "brazil", value: "Brazil" },
  { key: "france", value: "France" },
  { key: "united_kingdom", value: "United Kingdom" },
  { key: "finland", value: "Finland" },
]);
// Textos que serão exibidos
let texts = reactive([{}]);

let displayedTexts = reactive(Array(25).fill(""));
let digitando = reactive(Array(25).fill([""]));

// Array para armazenar as cores atuais de fundo
let backgroundColors = reactive(Array(25).fill(colors[0]));
// Estilos de transição aleatória
let transitionStyles = reactive(Array(25).fill(""));

let countriesNames = reactive([]);

let direction = reactive("");
function randomTransitionDirection() {
  //const directions = ["to top", "to bottom", "to left", "to right"];
  let slideDirections = ["slide-up", "slide-down","slide-left", "slide-right"];  //, 
  direction = slideDirections[Math.floor(Math.random() * slideDirections.length)];
  //return directions[Math.floor(Math.random() * directions.length)];
  return slideDirections[Math.floor(Math.random() * slideDirections.length)];

}

function randomTimeType(params) {
  return Math.floor(Math.random() * 5) + 1;
}


function textoParaLink(index, text) {
  // console.log("0=======: ", text, "---===-=-=-=-: ", index);
  if (index && text) {
    return text[index];
  }
}
// Função para o efeito de digitação
function typeText(index, text, callback) {
console.log('randomTimeType(): ', randomTimeType())

  //console.log("--->>>: ", index, "===>>", text);
  let currentIndex = 0;
  displayedTexts[index] = ""; // Reinicia o texto
  digitando[index] = true;
  function typeNextChar() {
    if (currentIndex < text.length) {
      displayedTexts[index] += text[currentIndex]; // Adiciona o próximo caractere
      currentIndex++;
      setTimeout(typeNextChar, randomTimeType() * 100); // Controla a velocidade da digitação
    } else if (callback) {
      digitando[index] = false;
      callback(); // Quando terminar de digitar, executa o callback
    }
  }

  typeNextChar(); // Inicia a digitação
}

// Função para iniciar a digitação e alterar o fundo
function startTypingAnimation() {
  for (let index = 0; index < displayedTexts.length; index++) {
    (function animateCard(i) {
      let randomText = getRandomText();
      textoParaLink(index, randomText);
      typeText(i, randomText, () => {
        // Escolhe uma nova cor e transição aleatória para o background
        const nextColor = getRandomColor();
        const transitionDirection = randomTransitionDirection();

        // Define o estilo de transição baseado na direção
        transitionStyles[
          i
        ] = `background-image 1s ease-in-out, background-size 1s, background-position 1s`;
        backgroundColors[i] = nextColor;

        // Após o delay, inicia a próxima frase aleatória
        setTimeout(() => {
          animateCard(i); // Repete a animação para o mesmo card
        }, 500); // Pequeno atraso após a troca de cor
      });
    })(index);
  }

  // Reseta a animação após um tempo
  setTimeout(() => {}, backgroundColors.length * 300 + 2000); // Espera todas as animações terminarem
}

// Função para pegar uma cor aleatória
function getRandomColor() {
  return colors[Math.floor(Math.random() * colors.length)];
}

// Função para pegar um texto aleatório
function getRandomText() {
  return texts[Math.floor(Math.random() * texts.length)];
}

function fetchData(updateCountry) {
  axios
    .get("/api/trends/hottrends/visualize/internal/data")
    .then(async (response) => {
      texts = await response.data[selectedCountry.value];
    })
    .finally(() => {
      if (!updateCountry) {
        startTypingAnimation();
      }
    })
    .catch((error) => console.log("Error:", error));
}

onBeforeMount(() => {
  selectedCountry.value = "brazil";
  fetchData(false);
});
</script>

<style lang="scss" scoped>
.card-grid {
  display: grid;
  grid-template-columns: repeat(5, 1fr); /* 5 colunas de igual tamanho */
  grid-template-rows: repeat(5, 1fr); /* 5 linhas de igual tamanho */
  width: 100vw;
  height: 100vh;
  gap: 0;
  padding: 0;
  margin: 0;
  box-sizing: border-box;
}

/* Estilo dos cards individuais */
.card {
  display: flex;
  justify-content: left;
  align-items: center;
  // transition: background-color 1s ease, transform 0.5s ease;
  font-family: monospace;
  font-size: 1.8rem;
  color: white;
  background-position: center;
}

.displayed-text {
  overflow: hidden;
  position: relative;
  float: left;
  transition: width 0s, height 0s;
  text-shadow: 0 0.025em 0 rgba(0, 0, 0, 0.1);
  word-wrap: break-word;
  padding-left: 0.5em;
  padding-right: 0.5em;
  color: white;
  text-decoration: none;
}

p {
  margin: 0;
}

/* Seleciona país */
.text-country {
  float: left;
  position: absolute;
  align-items: bottom;
  left: 22px;
  bottom: 20px;
  color: #fff;
  font-size: 13px;
  position: absolute;
  text-shadow: 0 1px 0 rgba(0, 0, 0, 0.2);
  z-index: 3;
  // font-family: "Roboto", Arial, sans-serif;
}

.card-container {
  display: inline-block;
  position: relative;
  height: 1em;
}

button {
  position: absolute;
}


/* UP */
.slide-up-enter-active,
.slide-up-leave-active {
  transition: all 0.5s ease-out;
}

.slide-up-enter-from {
  opacity: 0;
  transform: translateY(30px);
}

.slide-up-leave-to {
  opacity: 0;
  transform: translateY(-30px);
}

/* DOWN */
.slide-down-enter-active,
.slide-down-leave-active {
  transition: all 0.5s ease-out;
}

.slide-down-enter-from {
  opacity: 0;
  transform: translateY(-30px);
}

.slide-down-leave-to {
  opacity: 0;
  transform: translateY(30px);
}

/* LEFT */
.slide-left-enter-active,
.slide-left-leave-active {
  transition: all 0.5s ease-out;
}

.slide-left-enter-from {
  opacity: 0;
  transform: translateX(30px);
}

.slide-left-leave-to {
  opacity: 0;
  transform: translateX(-30px);
}

/* RIGHT */
.slide-right-enter-active,
.slide-right-leave-active {
  transition: all 0.5s ease-out;
}

.slide-right-enter-from {
  opacity: 0;
  transform: translateX(-30px);
}

.slide-right-leave-to {
  opacity: 0;
  transform: translateX(30px);
}



/* Esconde o botão de seleção, mas mantém a funcionalidade */
// .hidden-selects {
//   appearance: none; /* Remove o botão do select */
//   -webkit-appearance: none;
//   -moz-appearance: none ;
//   border: none; /* Remove a borda */
//   background: transparent; /* Fundo transparente */
//   width: 0; /* Define a largura como 0 */
//   height: 0; /* Define a altura como 0 */
//   position: absolute; /* Remove da estrutura visual */
// }
</style>
