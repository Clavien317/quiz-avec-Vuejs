<script setup>
import HelloWorld from './components/HelloWorld.vue'
import {ref,computed} from "vue"


const questions = ref([
  {
    question : "Vue js c'est quoi?",
    reponse:1,
    options:
    [
      'Librairie js',
      'Front-end framework js',
      'back-end framework js'
    ],
    selected: null
  },
  {
    question : "Expressjs c'est quoi?",
    reponse:2,
    options:
    [
      'Librairie js',
      'Front-end framework js',
      'back-end framework Node js'
    ],
    selected: null
  },
  {
    question : "Axios c'est quoi?",
    reponse:1,
    options:
    [
      'Librairie js',
      'Front-end API',
      'UI/UX'
    ],
    selected: null
  }

])


const quizzcompleted  =ref(false)
const currentQuest = ref(0)
const score = computed(()=>
{
  let value=0
  questions.value.map(q=>
  {
    if(q.selected ==q.reponse)
    {
      value++
    }
  })
  return value
})



const getCurrentquest = computed(()=>
{
  let quest = questions.value[currentQuest.value]
  quest.index = currentQuest.value

  return quest
})


const setAnswer =evt=>
{
  questions.value[currentQuest.value].selected=evt.target.value

  evt.target.value=null
}


const nextQuest=()=>
{
  if(currentQuest.value <questions.value.length -1)
  {
    currentQuest.value++

  }else
  {
    quizzcompleted.value = true
  }
}

</script>

<template>
<main class="app">
  <h1>Quiz app</h1>

  <section class="quiz" v-if="!quizzcompleted">
    <div class="quiz-info">
      <span class="question">{{getCurrentquest.question}}</span>
      <span class="score">Score {{score}}/{{questions.length}}</span>


    </div>

    <div class="option">
      <label v-for="(option,index) in getCurrentquest.options"
      :key="index" 
      :class="`option ${
        getCurrentquest.selected== index ? 
        index==getCurrentquest.reponse ?
         'correct' 
         :'wrong'
         :''
        }
        ${
          getCurrentquest.selected !=null &&index != getCurrentquest.selected
          ? 'disabled'
          :''
        }`">

        <input type="radio" :name="getCurrentquest.index" :value="index" id=""
        v-model="getCurrentquest.selected"
        :disabled="getCurrentquest.selected"
        @change="setAnswer">
        <span>{{option}}</span>
      </label>
    </div>

    <button @click="nextQuest" :disabled="!getCurrentquest.selected">
      {{ currentQuest.index ==questions.length -1
      ? 'Fin'
      :getCurrentquest.selected == null
         ? 'Select an option'
        : 'Next question' }}
    </button>
  </section>

  <section v-else>
    <hr>
    <h3>Vous avez fini la test !</h3>
    <br>
    <br>
    <h4>votre score <br> {{score}}/{{questions.length}}</h4>
  </section>
</main>
</template>

<style scoped>
.app
{
  display: flex;
  flex-direction: column;
  align-items: center;
  padding: 2rem;
  min-height: 10vh;
}

h1
{
  font-size: 2rem;
  margin-bottom: 2rem
}

.quiz
{
  background-color: blue;
  padding: 2rem;
  width: 100%;
  max-width: 640px;
  border-radius: 0.5rem;
}

.quiz-info
{
  display: flex;
  justify-content: space-between;
  margin-bottom: 1rem;
}

.quiz-info .question
{
  color: rgb(168, 160, 160);
  font-size: 1.25rem;
}

.quiz-info .score
{
  color: white;
  font-size: 1.5rem;
}

.options
{
  margin-bottom: 1rem;
}

.option
{
  display: block;
  padding: 1rem;
  background-color: rgb(40, 32, 203);
  margin-bottom: 0.5rem;
  border-radius: 0.5rem;
  cursor: pointer;
}

.option:hover
{
  background-color: rgb(12, 7, 58);
}

.option .correct
{
  background-color: green;
}

.option .wrong
{
  background-color: red;
}

.option:last-of-type
{
  margin-bottom: 0;
}

.option .disabled
{
  opacity: 0.5 ;
}

.option input
{
  display: none;
}

button
{
  appearance: none;
  outline: none;
  border: none;
  cursor: pointer;

  padding: 0.5rem 1rem;
  background-color: rgb(22, 6, 108);
  color: white;
  text-transform: uppercase;
  font-size: 1.3rem;
  border-radius: 0.5rem;
  font-weight: 700;
}

button:disabled
{
 opacity: 0.5; 
}

h2
{
  font-size: 2rem;
  margin-bottom: 2rem;
  text-align: center;
}

p
{
  color: antiquewhite;
  font-size: 1.25rem;
}

</style>
