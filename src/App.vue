<script setup>
import { ref, computed } from 'vue'



const questions = [
  {
    texte: "Quel est le nom de la ville considérée comme la ville la plus sainte de l'Islam ?",
    image: "/images/kaaba.jpg",
    choix: ["Makkah", "Médine", "Dubaï"],
    bonneReponse: 0,
    descriptionImage: "La Kaaba à Makkah"
  },
  {
    texte: "Quelle civilisation antique est célèbre pour avoir construit les pyramides de Gizeh ?",
    image: "/images/pyramide.webp",
    choix: ["Égypte", "Maroc", "Italie"],
    bonneReponse: 0,
    descriptionImage: "Les pyramides de Gizeh"
  },
  {
    texte: "Quel est le monument emblématique construit pour l'Exposition universelle de 1889 ?",
    image: "/images/tour.jpg",
    choix: ["Londres", "Paris", "Rome"],
    bonneReponse: 1,
    descriptionImage: "La Tour Eiffel à Paris"
  },
  {
    texte: "Dans quel pays se situe la ville de Dubaï ?",
    image: "/images/dubai.jpg",
    choix: ["Qatar", "Émirats arabes unis", "Égypte"],
    bonneReponse: 1,
    descriptionImage: "La ville de Dubaï"
  },
  {
    texte:  "Dans quelle capitale européenne peut-on trouver Big Ben ?",
    image:"/images/bigben.png",
    choix: ["Paris", "Londres", "Rome"],
    bonneReponse: 1,
    descriptionImage: "Big Ben à Londres"
  }
]

const indexQuestion = ref(0)
const score = ref(0)
const reponseChoisie = ref(false)
const termine = ref(false)
const bonneReponse = ref(false)

const questionActuelle = computed(() => {
  return questions[indexQuestion.value]
})

const progression = computed(() => {
  return indexQuestion.value + (reponseChoisie.value ? 1 : 0)
})

function repondre(index) {
  if (reponseChoisie.value) {
    return
  }

  reponseChoisie.value = true

  if (index === questionActuelle.value.bonneReponse) {
    score.value++
    bonneReponse.value = true
  } else {
    bonneReponse.value = false
  }
}

function questionSuivante() {
  if (indexQuestion.value < questions.length - 1) {
    indexQuestion.value++
    reponseChoisie.value = false
    bonneReponse.value = false
  } else {
    termine.value = true
  }
}

function rejouer() {
  indexQuestion.value = 0
  score.value = 0
  reponseChoisie.value = false
  bonneReponse.value = false
  termine.value = false
}
</script>

<template>
  <div class="quiz">

    <header>
      <h1>Quiz en images</h1>

      <div class="score">
        Score : {{ score }}/5
      </div>
    </header>

    <div class="progression">
      <p>Progression : {{ progression }}/5</p>

      <progress
        :value="progression"
        :max="questions.length">
      </progress>
    </div>

    <!-- Quiz -->
    <div v-if="!termine">

      <h2>
        Question {{ indexQuestion + 1 }} sur {{ questions.length }}
      </h2>

      <!-- Image -->
      <img
        :src="questionActuelle.image"
        :alt="questionActuelle.descriptionImage"
        class="image-question"
      >

      <!-- Question -->
      <h3>
        {{ questionActuelle.texte }}
      </h3>

      <!-- Les 3 choix -->
      <div class="choix">

        <button
          v-for="(choix, index) in questionActuelle.choix"
          :key="index"
          @click="repondre(index)"
          :disabled="reponseChoisie"
        >
          {{ choix }}
        </button>

      </div>

      <!-- Message -->
      <div v-if="reponseChoisie">

        <p v-if="bonneReponse">
          Bonne réponse !
        </p>

        <p v-else>
          Mauvaise réponse.
        </p>

        <button @click="questionSuivante">
          {{ indexQuestion === questions.length - 1
            ? 'Voir le résultat'
            : 'Question suivante' }}
        </button>

      </div>

    </div>

    <!-- Résultat final -->
    <div v-else class="resultat">

      <h2>Quiz terminé !</h2>

      <p>
        Votre score final :
      </p>

      <strong>
        {{ score }}/5
      </strong>

      <br><br>

      <button @click="rejouer">
        Rejouer
      </button>

    </div>

  </div>
</template>
<style scoped>

.quiz {
  max-width: 750px;
  margin: 40px auto;
  padding: 30px;
  text-align: center;
  font-family: Arial, sans-serif;
  background: white;
  border-radius: 25px;
  box-shadow: 0 10px 30px rgba(0, 0, 0, 0.15);
}

html, body, #app {
  margin: 0;
  min-height: 100%;
}

body {
  min-height: 100vh;
  background: linear-gradient(135deg, #ffeaa7, #ff9ff3, #74b9ff);
}
header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  background: #ffe066;
  padding: 15px 20px;
  border-radius: 15px;
}

h1 {
  color: #6c3483;
  margin: 0;
}

.score {
  font-weight: bold;
  font-size: 20px;
  color: #e84393;
}


.progression {
  margin: 20px 0;
  color: #6c3483;
  font-weight: bold;
}

progress {
  width: 100%;
  height: 20px;
  accent-color: #ff7675;
}

h2 {
  color: #0984e3;
}

h3 {
  color: #6c3483;
  font-size: 22px;
}


.image-question {
  width: 400px;
  height: 250px;
  object-fit: contain;
  margin: 20px 0;
  border-radius: 20px;
  box-shadow: 0 8px 20px rgba(0, 0, 0, 0.15);
}


.choix {
  display: flex;
  justify-content: center;
  gap: 15px;
  margin: 25px 0;
  flex-wrap: wrap;
}


button {
  padding: 13px 22px;
  border: none;
  border-radius: 12px;
  cursor: pointer;
  background-color: #74b9ff;
  color: white;
  font-size: 16px;
  font-weight: bold;
  transition: 0.3s;
}

button:hover {
  background-color: #0984e3;
  transform: scale(1.05);
}

button:disabled {
  cursor: not-allowed;
  opacity: 0.6;
  transform: none;
}

/* Résultat */
.resultat {
  margin-top: 30px;
  padding: 25px;
  background-color: #fff3cd;
  border-radius: 20px;
}

.resultat h2 {
  color: #e84393;
}

.resultat strong {
  font-size: 45px;
  color: #00b894;
}

/* Responsive */
@media (max-width: 600px) {

  .quiz {
    margin: 20px;
    padding: 20px;
  }

  header {
    flex-direction: column;
    gap: 10px;
  }

  .image-question {
    width: 100%;
    height: 220px;
  }

  .choix {
    flex-direction: column;
  }

  button {
    width: 100%;
  }
  
}

</style>