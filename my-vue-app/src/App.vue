<template>
  <div class="chat-container">
    <div class="chat-window">
      <div class="messages" ref="messages">
        <p v-for="message in messages" :key="message.id">
          <span v-if="message.isBot">Бот: </span>{{ message.text }}
        </p>
      </div>
      <div class="input-container">
        <input type="text" v-model="inputText" @keyup.enter="sendMessage" />
        <button @click="sendMessage">Отправить</button>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name:'app',
  data() {
    return {
      messages: [],
      inputText: "",
    };
  },
  methods: {
    addButtons() {
      ["Заказать пиццу", "Установить будильник", "Вывести погоду"].forEach((buttonText) => {
        this.messages.push({ id: this.messages.length, text: buttonText, isBot: true });
      });
    },
    sendMessage() {
      if (this.inputText.trim() === "") return;
      this.messages.push({ id: this.messages.length, text: this.inputText, isBot: false });
      this.inputText = "";
      this.handleUserMessage(this.messages[this.messages.length - 1].text);
    },
    handleUserMessage(userMessage) {
      this.messages.push({ id: this.messages.length, text: this.getBotResponse(userMessage), isBot: true });
    },
    getBotResponse() {
      // Здесь должена быть логика обработки сообщений пользователя
      return "Хорошо, я закажу пиццу. Что еще могу сделать?";
    },
  },
  mounted() {
    this.messages.push({ id: this.messages.length, text: "Бот: Привет! Что я могу для Вас сделать?", isBot: true });
    this.addButtons();
  },
};
</script>

<style scoped>
.chat-container {
  display: flex;
  justify-content: center;
  align-items: center;
  height: 100vh;
}

.chat-window {
  display: flex;
  flex-direction: column;
  width: 400px;
  padding: 20px;
  box-shadow: 0 1px 3px rgba(0, 0, 0, 0.12), 0 1px 2px rgba(0, 0, 0, 0.24);
}

.messages {
  flex-grow: 1;
  overflow-y: auto;
  padding: 10px;
}

.input-container {
  display: flex;
  margin-top: 10px;
}

.input-container input {
  flex-grow: 1;
  padding: 5px;
}

.input-container button {
  margin-left: 10px;
  padding: 5px 10px;
}
</style>