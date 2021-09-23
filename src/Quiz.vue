<template>
  <div>
    <div class="nav-bar"></div>
    <!-- First div to loop through questions and their respective answer choices -->
    <div v-if="!quizEnd">
      <h1 class="header">Question {{ currentQuest }}</h1>
      <hr size="1" width="100%" color="gray" />
      <h3 class="header">{{ currentQuestion.text }}</h3>
      <div>
        <!-- Display each question with their respective answer choices -->
        <label
          class="text"
          v-for="answer in currentQuestion.answers"
          :key="answer"
        >
          <input
            name="choices"
            id="answer"
            type="radio"
            v-model="selectedAnswer"
            :value="answer"
          />
          {{ answer }}
          <br />
        </label>
      </div>

      <!-- Two different buttons to display depending on 
      if the quiz is on the final question or not -->
      <button
        v-if="!(currentQuest === quizLength)"
        @click="toNextQuest"
        :disabled="!selectedAnswer"
        :class="{ disabledButton: !selectedAnswer }"
      >
        Next Question
      </button>
      <button
        v-else
        @click="toNextQuest"
        :disabled="!selectedAnswer"
        :class="{ disabledButton: !selectedAnswer }"
      >
        Submit
      </button>
    </div>

    <!-- Second div to display final screen when all questions are completed -->
    <div v-else>
      <h1 class="header">Thank you for your submission!</h1>
      <p class="text">The following is a summary of your answers:</p>
      <hr size="1" width="100%" color="gray" />
      <br />
      <div
        v-for="(finalResponse, index) in finalAnswers"
        :key="finalResponse.question"
      >
        <h4 class="header">
          Question {{ index + 1 }}:
          {{ finalResponse.question }}
        </h4>
        <p class="text">{{ finalResponse.answer }}</p>
        <br />
      </div>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    questions: {
      type: Array,
      required: true,
    },
  },
  data() {
    return {
      question: this.questions[0],
      quizLength: this.questions.length,
      currentQuest: 1,
      selectedAnswer: "",
      finalAnswers: [],
    };
  },
  methods: {
    toNextQuest() {
      /* Store answers in finalAnswers array for 
      displaying on final page later */
      this.finalAnswers.push({
        question: this.currentQuestion.text,
        answer: this.selectedAnswer,
      });
      /* Advance currentQuest counter to next question and 
      reset selectedAnswer so it can be used in the next iteration */
      this.currentQuest += 1;
      this.selectedAnswer = "";
    },
  },
  computed: {
    currentQuestion() {
      return this.questions[this.currentQuest - 1];
    },
    quizEnd() {
      return this.currentQuest > this.questions.length;
    },
  },
};
</script>

<style scoped>
.header {
  font-family: Helvetica;
  color: #282828;
  margin: 20px;
}
.text {
  font-family: Arial;
  color: #282828;
  margin: 20px;
}

.nav-bar {
  background-color: #cb410b;
  height: 60px;
  margin-bottom: 15px;
}
button {
  margin: 20px;
  margin-top: 30px;
  border: none;
  border-radius: 4px;
  background-color: #00a693;
  color: white;
  height: 40px;
  width: 150px;
  font-size: 14px;
}
.disabledButton {
  background-color: #d8d8d8;
}
</style>