
<template>
  <div class="chat-container">

    <!-- Name Input window -->
    <div v-if="!connect">
        <div class="modal-background">
          <div class="modal-content">
            <form @submit.prevent="handleSubmit">
              <h3> Enter your name to start chatting </h3>
              <input type="text" v-model="username" placeholder="Enter your name" />
              <br>
              <button type="submit"> Connect </button>
            </form>
          </div>
        </div>
      </div>

    <div v-if="connect" class="chat-window">
      <div class="messages-container">
        <!-- <ul> -->
          <div v-for="(val, index) in messages" :key="index" :class="[val.username === username ? 'left-bubble' : 'right-bubble']">
              <b>{{ val.username }}</b>: <em>{{ val.message }}</em>
          </div>
        <!-- </ul> -->
      </div>

      <div class="chat-input">
          <form @submit.prevent="handleMessageSubmit(username,text)">
          <input type="text" v-model="text" placeholder="Enter your message" />
          <button type="submit">Send</button>
          </form>
      </div>
    </div>
  </div>
</template>

  <script>
  // import { useWebSocket } from '@vueuse/core'
  import { ref,} from 'vue';
  
  export default {
    name: 'ChatComponent',
    setup() {
      const username = ref('')
      const connect = ref(false)
      const text = ref('')
      const messages = ref([])
      const socket = new WebSocket('ws://localhost:3000')


      const handleSubmit = () => {
        if(username.value.length > 0) {
          connect.value = true
        }
      }

    const handleMessageSubmit = (username,text) => {
      const messageData = { username: username, message: text};
      socket.send(JSON.stringify(messageData))
      messages.value.push(messageData)
      text = ""
    }
  
    socket.onmessage = (event) => {
      const message = JSON.parse(event.data);
      messages.value.push(message);
  }

  
      return {
        text,
        connect,
        username,
        messages,
        handleSubmit,
        handleMessageSubmit
      }
    },
  }
  </script>
  
  <style scoped>
 

 .chat-container {
  height: 100%;
    display: flex;
    flex-direction: column;
    justify-content: space-between;
}
.modal-background {
    position: fixed;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    background-color: rgba(0, 0, 0, 0.5);
    display: flex;
    align-items: center;
    justify-content: center;
}
.modal-content {
    background-color: #3d2aac;
    padding: 15px;
    border-radius: 5px;
    box-shadow: 0px 0px 10px #333;
    text-align: center;
    color: white;
}
.modal-content input{
    padding: 10px;
    border-radius: 5px;
    border: none;
    margin-bottom: 10px;
}
.modal-content button{
    padding: 10px;
    border-radius: 5px;
    border: none;
    background-color: #9b59b6;
    color: white;
}
.chat-window {
    display: flex;
    flex-direction: column;
}

.chat-messages{
    padding: 10px;
    margin-bottom: 10px;
    border-radius: 5px;
}

.chat-input {
  background-color: #f2f2f2;
    padding: 30px;
    display: flex;
    align-items: center;
    position: fixed;
    bottom: 0;
    width: 100%;
}
.chat-input input[type="text"] {
  flex: 1;
    padding: 15px;
    border: none;
    border-radius: 5px 0px 0px 5px;
}
.chat-input button {
  background-color: #3d2aac;
    color: #fff;
    padding: 15px;
    border: none;
    border-radius: 0px 5px 5px 0px;
}

.right-bubble{
  text-align: right;
    background-color: #3d2aac;
    border-radius: 10px 10px 0px 10px;
    padding: 10px 15px;
    display: inline-block;
    word-wrap: break-word;
    margin: 10px;
    float: right;
    color: white;
}
  
.left-bubble{
  text-align: left;
    background-color:#9b59b6;
    border-radius: 10px 10px 10px 0px;
    padding: 10px 15px;
    word-wrap: break-word;
    display: inline-block;
    margin: 10px;
    float: left;
    color: black;
}
  </style>