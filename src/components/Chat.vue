<template>
  <div class="chat-container">

    <!-- Name Input window -->
    <div v-if="!connect">
        <div class="modal-background">
          <div class="modal-content">
            <form @submit.prevent="handleConnect">
              <h3> Enter your name to join chat </h3>
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
          <!-- Use a v-for directive to iterate over the messages array and display each message -->
          <div v-for="(val, index) in messages" :key="index" :class="[val.username === username ? 'left-bubble' : 'right-bubble']">
              <!-- Use mustache syntax to interpolate the username and message properties of each message object -->
              <b>{{ val.username }}</b>: <em>{{ val.message }}</em>
          </div>
        <!-- </ul> -->
      </div>

      <div class="chat-input">
          <form @submit.prevent="sendMessage()">
          <input type="text" v-model="text" placeholder="Write message..." />
          <button type="submit"><i class="bi bi-send "></i> </button>
          </form>
      </div>
    </div>
  </div>
</template>

<!-- Use the 'setup' function from the Vue Composition API -->
<script>
  import { ref} from 'vue';
  
  export default {
    name: 'ChatComponent',
    setup() {
      // Declare reactive variables using the 'ref' function
      const username = ref(null)
      const connect = ref(false)
      const text = ref(null)
      const messages = ref([])
      // The use of the WebSocket API 
      const socket = new WebSocket('ws://localhost:3000')

      
      const handleConnect = () => {
        if(username.value.length > 0) {
          connect.value = true
        }
      }

      const sendMessage = () => {
        const messageData = { username: username.value, message: text.value};
        // Send the message data to the server using WebSockets
        socket.send(JSON.stringify(messageData))
        // Add the message data to the messages array
        messages.value.push(messageData)
        // Clear the text input
        text.value = null;
      }
  
      // Define a WebSocket 'onmessage' event handler to receive messages from the server
      socket.onmessage = (event) => {
        const message = JSON.parse(event.data);
        messages.value.push(message);
      }
  
      // Return the reactive variables and event handlers to be used in the template
      return {
        text,
        connect,
        username,
        messages,
        handleConnect,
        sendMessage
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
.message-container{
    display: inline-block;
    padding: 0 0 0 10px;
    vertical-align: top;
}

.right-bubble{
    background-color: #3d2aac;
    border-radius: 10px 10px 10px 0px;
    padding: 10px 15px;
    word-wrap: break-word;
    margin: 10px;
    float: right;
    color: white;
    width:50%;
}
  
.left-bubble{
  text-align: left;
    background-color:#9b59b6;
    border-radius: 10px 10px 0px 10px;
    padding: 10px 15px;
    word-wrap: break-word;
    margin: 10px;
    float: left;
    color: white;
    width: 50%;
}
  </style>