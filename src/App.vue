<script setup>
import Quiz from "./components/Quiz.vue";
</script>

<template>
  <Quiz />
</template>

<style>
@import url("https://fonts.googleapis.com/css2?family=Inter:wght@400;600;700&display=swap");

:root {
  --accent: #61dafb;
  --feedback-correct: #2ecc71;
  --feedback-wrong: #e74c3c;
  --feedback-correct-light: rgba(46, 204, 113, 0.1);
  --feedback-wrong-light: rgba(231, 76, 60, 0.1);
  --bg-primary: #20232a;
  --bg-secondary: #282c34;
  --border-color: #3c3f4a;
  --text-primary: #f1f1f1;
  --text-secondary: #a0aec0;
}

* {
  box-sizing: border-box;
  margin: 0;
  padding: 0;
}

body {
  background: var(--bg-primary);
  color: var(--text-primary);
  font-family: "Inter", system-ui, -apple-system, BlinkMacSystemFont, "Segoe UI",
    Roboto, "Helvetica Neue", Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  padding: 1rem;
}
</style>
