# Quiz de React

![Vue.js](https://img.shields.io/badge/vuejs-%2335495e.svg?style=for-the-badge&logo=vuedotjs&logoColor=%234FC08D)
![JavaScript](https://img.shields.io/badge/javascript-%23323330.svg?style=for-the-badge&logo=javascript&logoColor=%23F7DF1E)
![Vite](https://img.shields.io/badge/vite-%23646CFF.svg?style=for-the-badge&logo=vite&logoColor=white)

Um quiz sobre React, mas com um plot twist: foi feito inteiramente em **Vue.js**.

## Funcionalidades

- **Perguntas com Temporizador:** Cada pergunta possui um cronômetro regressivo de 30 segundos, adicionando um elemento de desafio e urgência ao quiz.
- **Feedback Instantâneo:** Ao selecionar uma resposta, a opção é imediatamente destacada em verde (correta) ou vermelho (incorreta), proporcionando feedback claro e imediato.
- **Acompanhamento de Progresso:** Uma barra de progresso no topo e um contador (ex: "Questão 1 de 20") permitem que o usuário acompanhe seu avanço de forma intuitiva.
- **Transições Suaves:** Animações elegantes são utilizadas na transição entre as perguntas, criando uma experiência de usuário fluida e profissional.
- **Design Moderno:** A interface é limpa, com um tema escuro e cores que remetem à identidade visual do React, garantindo uma ótima experiência de uso.

## Tecnologias

- **Vue.js:** O projeto foi desenvolvido utilizando a **Composition API** com a sintaxe `<script setup>`, aproveitando o que há de mais moderno no ecossistema Vue.
- **JavaScript:** Utilizado para toda a lógica do quiz, incluindo gerenciamento de estado, tempo e pontuação.
- **Vite:** Utilizado como a ferramenta de build e servidor de desenvolvimento, garantindo uma experiência de desenvolvimento extremamente rápida com Hot Module Replacement (HMR).
- **HTML5 & CSS3:** Para a estruturação e estilização do componente, incluindo o uso de variáveis CSS para um tema coeso e animações para as interações.

## Instalação

```bash
git clone https://github.com/caiolucasbittencourt/react-quiz
cd react-quiz
npm install
npm run dev
