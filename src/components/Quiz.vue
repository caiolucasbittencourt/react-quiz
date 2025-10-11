<script setup>
import { ref, computed, onMounted, watch } from "vue";

const isChangingQuestion = ref(false);
const sleep = (ms) => new Promise((resolve) => setTimeout(resolve, ms));

const questions = [
  {
    question: "O que é JSX?",
    options: [
      "Uma linguagem de template para JavaScript",
      "Uma extensão de sintaxe para JavaScript",
      "Uma biblioteca de gerenciamento de estado",
      "Uma função do JavaScript",
    ],
    answer: 1,
  },
  {
    question: "Qual destes é um componente funcional válido?",
    options: [
      "function MyComponent() { return <div>Hello</div>; }",
      "class MyComponent extends React { return <div>Hello</div>; }",
      "component MyComponent() { <div>Hello</div> }",
      "func MyComponent() => <div>Hello</div>",
    ],
    answer: 0,
  },
  {
    question:
      "Como os dados são passados de um componente pai para um componente filho?",
    options: [
      "Através do estado (state)",
      "Através de adereços (props)",
      "Através de contexto (context)",
      "Através de eventos (events)",
    ],
    answer: 1,
  },
  {
    question: "A principal diferença entre 'state' e 'props' é:",
    options: [
      "Props são mutáveis, state é imutável",
      "State é passado do pai, props são gerenciadas internamente",
      "Props são para dados externos, state é para eventos",
      "State é gerenciado dentro do componente, props são recebidas do pai",
    ],
    answer: 3,
  },
  {
    question:
      "Qual hook é usado para adicionar estado a um componente de função?",
    options: ["useEffect", "useContext", "useState", "useReducer"],
    answer: 2,
  },
  {
    question: "Como você aplica classes CSS em JSX?",
    options: [
      "<div class='container'>",
      "<div className='container'>",
      "<div classNameName='container'>",
      "<div class-name='container'>",
    ],
    answer: 1,
  },
  {
    question:
      "Qual é a forma correta de lidar com eventos de clique em um botão?",
    options: [
      "<button onclick={handleClick}>",
      "<button onClick={handleClick}>",
      "<button on-click={handleClick}>",
      "<button event:click={handleClick}>",
    ],
    answer: 1,
  },
  {
    question:
      "Para renderizar listas de elementos, o que é essencial para cada item?",
    options: [
      "Um atributo 'id' único",
      "Uma classe CSS única",
      "Um atributo 'key' único",
      "Um índice numérico",
    ],
    answer: 2,
  },
  {
    question:
      "Como você pode renderizar um componente `MyComponent` apenas se a variável `show` for verdadeira?",
    options: [
      "<MyComponent v-if={show} />",
      "{show && <MyComponent />}",
      "<show><MyComponent /></show>",
      "if (show) return <MyComponent />",
    ],
    answer: 1,
  },
  {
    question: "Para que serve o hook `useEffect`?",
    options: [
      "Para renderizar elementos condicionalmente",
      "Para criar variáveis de estado",
      "Para executar efeitos colaterais em componentes",
      "Para otimizar a performance do componente",
    ],
    answer: 2,
  },
  {
    question: "O que acontece quando você altera o state de um componente?",
    options: [
      "Nada, o React ignora mudanças no state",
      "O componente é re-renderizado",
      "O state é alterado, mas não o DOM",
      "O React atualiza apenas o CSS",
    ],
    answer: 1,
  },
  {
    question:
      "Como você pode evitar que um formulário recarregue a página ao enviar?",
    options: [
      "Usando event.preventDefault() no onSubmit",
      "Adicionando type='button' ao submit",
      "Chamando handleSubmit() manualmente",
      "Não é possível evitar o reload",
    ],
    answer: 0,
  },
  {
    question:
      "Qual ferramenta é mais comumente usada para criar um novo projeto React?",
    options: [
      "React CLI",
      "Webpack",
      "Vite / Create React App",
      "npm init react",
    ],
    answer: 2,
  },
  {
    question: "O que o `Context API` do React ajuda a resolver?",
    options: [
      "O roteamento entre páginas",
      "A execução de código assíncrono",
      "O problema de 'prop drilling'",
      "A estilização de componentes",
    ],
    answer: 2,
  },
  {
    question: "Qual hook permite acessar valores de um Context?",
    options: ["useState", "useContext", "useReducer", "useEffect"],
    answer: 1,
  },
  {
    question: "O que é um 'componente controlado' em React?",
    options: [
      "Um componente que gerencia seu próprio estado interno",
      "Um componente que recebe dados via props e os altera diretamente",
      "Um componente cujo valor do input é controlado pelo state",
      "Um componente que não usa hooks",
    ],
    answer: 2,
  },
  {
    question:
      "Qual das opções abaixo é uma forma correta de atualizar o state?",
    options: [
      "state = novoValor",
      "setState(novoValor)",
      "this.state = novoValor",
      "state.set(novoValor)",
    ],
    answer: 1,
  },
  {
    question: "Qual hook é ideal para gerenciar múltiplos estados complexos?",
    options: ["useEffect", "useReducer", "useState", "useMemo"],
    answer: 1,
  },
  {
    question:
      "Qual hook é usado para memorizar valores ou funções para otimização?",
    options: ["useMemo", "useCallback", "useEffect", "useRef"],
    answer: 0,
  },
  {
    question: "O que o hook `useRef` permite fazer?",
    options: [
      "Criar estado local",
      "Referenciar elementos DOM ou manter valores persistentes",
      "Executar efeitos colaterais",
      "Atualizar props",
    ],
    answer: 1,
  },
];

const QUESTION_TIME_SECONDS = 30;
const current = ref(0);
const score = ref(0);
const showResult = ref(false);
const timeLeft = ref(QUESTION_TIME_SECONDS);
const selectedIndex = ref(null);
let intervalId = null;

const progressPercent = computed(() =>
  Math.round(((current.value + 1) / questions.length) * 100)
);

const resultFeedback = computed(() => {
  const percentage = (score.value / questions.length) * 100;
  if (percentage < 40)
    return { message: "Padawan em treinamento, continue praticando a Força!" };
  if (percentage < 75)
    return { message: "Cavaleiro Jedi em ascensão, bom trabalho!" };
  return {
    message: "Mestre Jedi da Força! Que a sabedoria esteja sempre com você!",
  };
});

const startTimer = () => {
  timeLeft.value = QUESTION_TIME_SECONDS;
  clearTimer();
  intervalId = setInterval(() => {
    if (timeLeft.value <= 1) {
      clearTimer();
      handleTimeout();
    } else {
      timeLeft.value--;
    }
  }, 1000);
};

const clearTimer = () => {
  if (intervalId) clearInterval(intervalId);
};

const goToNextQuestion = async () => {
  isChangingQuestion.value = true;
  await sleep(800);

  if (current.value < questions.length - 1) {
    current.value++;
  } else {
    showResult.value = true;
  }
  selectedIndex.value = null;
  isChangingQuestion.value = false;
};

const handleAnswer = async (index) => {
  if (selectedIndex.value !== null) return;

  clearTimer();
  selectedIndex.value = index;
  if (index === questions[current.value].answer) score.value++;

  await sleep(600);
  await goToNextQuestion();
};

const handleTimeout = async () => {
  selectedIndex.value = -1;
  await sleep(600);
  await goToNextQuestion();
};

const restart = () => {
  clearTimer();
  current.value = 0;
  score.value = 0;
  showResult.value = false;
  selectedIndex.value = null;
  startTimer();
};

watch(current, () => {
  if (!showResult.value) startTimer();
});

onMounted(startTimer);
</script>

<template>
  <main class="quiz-container">
    <template v-if="!showResult">
      <header class="quiz-header">
        <div class="meta">
          <span class="q-number"
            >Questão {{ current + 1 }} de {{ questions.length }}</span
          >
          <div class="timer" :class="{ 'low-time': timeLeft <= 5 }">
            <svg
              xmlns="http://www.w3.org/2000/svg"
              width="24"
              height="24"
              viewBox="0 0 24 24"
              fill="none"
              stroke="currentColor"
              stroke-width="2"
              stroke-linecap="round"
              stroke-linejoin="round"
            >
              <circle cx="12" cy="12" r="10"></circle>
              <polyline points="12 6 12 12 16 14"></polyline>
            </svg>
            <span>{{ timeLeft }}s</span>
          </div>
        </div>
        <div class="progress-bar" aria-hidden>
          <div
            class="progress-fill"
            :style="{ width: `${progressPercent}%` }"
          />
        </div>
      </header>

      <Transition name="slide-fade" mode="out-in">
        <div
          v-if="isChangingQuestion"
          key="loader"
          class="inter-loader-container"
        >
          <svg
            class="react-logo"
            viewBox="-11.5 -10.23174 23 20.46348"
            xmlns="http://www.w3.org/2000/svg"
          >
            <circle cx="0" cy="0" r="2.05" fill="#61DAFB" />
            <g stroke="#61DAFB" stroke-width="1" fill="none">
              <ellipse rx="11" ry="4.2" />
              <ellipse rx="11" ry="4.2" transform="rotate(60)" />
              <ellipse rx="11" ry="4.2" transform="rotate(120)" />
            </g>
          </svg>
        </div>
        <section v-else class="quiz-body" :key="current">
          <h2 class="question">{{ questions[current].question }}</h2>
          <div class="options">
            <button
              v-for="(opt, i) in questions[current].options"
              :key="i"
              class="option"
              :class="{
                correct:
                  selectedIndex !== null && i === questions[current].answer,
                wrong: selectedIndex === i && i !== questions[current].answer,
                disabled: selectedIndex !== null,
              }"
              @click="handleAnswer(i)"
              type="button"
              :disabled="selectedIndex !== null"
            >
              <span class="opt-label">{{ String.fromCharCode(65 + i) }}</span>
              <span class="opt-text">{{ opt }}</span>
            </button>
          </div>
        </section>
      </Transition>
    </template>

    <Transition v-else name="slide-fade" mode="out-in">
      <section class="result">
        <p class="score">
          Você acertou <strong>{{ score }}</strong> de
          <strong>{{ questions.length }}</strong>
        </p>
        <p class="percent">
          {{ resultFeedback.message }}
        </p>
        <div class="result-actions">
          <button @click="restart" class="btn-restart">Tentar novamente</button>
        </div>
      </section>
    </Transition>
  </main>
</template>

<style scoped>
.slide-fade-enter-active {
  transition: all 0.3s ease-out;
}
.slide-fade-leave-active {
  transition: all 0.3s cubic-bezier(1, 0.5, 0.8, 1);
}
.slide-fade-enter-from,
.slide-fade-leave-to {
  transform: translateY(10px);
  opacity: 0;
}

@keyframes spin {
  from {
    transform: rotate(0deg);
  }
  to {
    transform: rotate(360deg);
  }
}
.react-logo {
  width: 100px;
  height: 100px;
  animation: spin 3s linear infinite;
}
.inter-loader-container {
  display: flex;
  justify-content: center;
  align-items: center;
  min-height: 50vh;
}

.quiz-container {
  width: 100%;
  max-width: 960px;
  margin: 2rem auto;
  background: none;
  border: none;
  padding: 0;
}
.quiz-header {
  margin-bottom: 3rem;
}
.meta {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 1.5rem;
  font-size: 1.1rem;
}
.progress-bar {
  width: 100%;
  height: 8px;
  background: var(--border-color);
  border-radius: 99px;
  overflow: hidden;
}
.progress-fill {
  height: 100%;
  background: var(--accent);
  transition: width 0.4s ease-in-out;
}
@keyframes tick {
  0%,
  100% {
    transform: rotate(0deg);
  }
  25% {
    transform: rotate(8deg);
  }
  75% {
    transform: rotate(-8deg);
  }
}
.timer {
  display: flex;
  align-items: center;
  gap: 0.5rem;
  font-weight: 700;
  color: var(--accent);
}
.timer svg {
  stroke: var(--accent);
  transition: stroke 0.3s ease;
}
.timer.low-time {
  color: var(--feedback-wrong);
}
.timer.low-time svg {
  stroke: var(--feedback-wrong);
}
.question {
  color: var(--text-primary);
  font-size: 1.5rem;
  font-weight: 700;
  margin-bottom: 3rem;
  line-height: 1.3;
  text-align: center;
}
.options {
  display: flex;
  flex-direction: column;
  gap: 1rem;
  max-width: 800px;
  margin: 0 auto;
}
.option {
  display: flex;
  align-items: center;
  gap: 1.25rem;
  background: transparent;
  border: 2px solid var(--border-color);
  color: var(--text-secondary);
  padding: 1.25rem;
  border-radius: 12px;
  cursor: pointer;
  transition: all 0.2s ease;
  font-size: 1.2rem;
  font-weight: 600;
  text-align: left;
}
.option:not(.disabled):not(.correct):not(.wrong):hover {
  border-color: var(--accent);
  color: var(--accent);
  background: rgba(97, 218, 251, 0.05);
}
.opt-label {
  display: flex;
  align-items: center;
  justify-content: center;
  width: 2.5rem;
  height: 2.5rem;
  border-radius: 50%;
  background: var(--border-color);
  color: var(--text-secondary);
  font-weight: 700;
  flex-shrink: 0;
  transition: all 0.2s ease;
}
.option:not(.disabled):not(.correct):not(.wrong):hover .opt-label {
  background: var(--accent);
  color: var(--bg-primary);
}
.option.disabled {
  cursor: not-allowed;
  opacity: 0.7;
}
.option.correct {
  border-color: var(--feedback-correct);
  background: var(--feedback-correct-light);
  color: var(--feedback-correct);
  opacity: 1;
}
.option.correct .opt-label {
  background: var(--feedback-correct);
  color: var(--text-primary);
}
.option.wrong {
  border-color: var(--feedback-wrong);
  background: var(--feedback-wrong-light);
  color: var(--feedback-wrong);
  opacity: 1;
}
.option.wrong .opt-label {
  background: var(--feedback-wrong);
  color: var(--text-primary);
}
.result {
  position: fixed;
  top: 0;
  left: 0;
  width: 100vw;
  height: 100vh;
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  text-align: center;
  background: rgba(0, 0, 0, 0.05);
  padding: 2rem;
  z-index: 100;
}

.result-title {
  font-size: 2.5rem;
  font-weight: 700;
  color: var(--text-primary);
  margin-bottom: 0.5rem;
}
.score {
  font-size: 1.25rem;
  color: var(--text-primary);
  margin-bottom: 0.5rem;
}
.score strong {
  color: var(--accent);
  font-weight: 700;
}
.percent {
  color: var(--text-secondary);
  margin-bottom: 2rem;
}
.btn-restart {
  background: var(--accent);
  border: none;
  color: var(--bg-primary);
  padding: 1rem 2rem;
  border-radius: 8px;
  cursor: pointer;
  font-weight: 700;
  font-size: 1.1rem;
  transition: all 0.3s ease;
}
.btn-restart:hover {
  filter: brightness(1.1);
}
</style>
