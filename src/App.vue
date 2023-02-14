<template>
  <div>
        <header> 
          <h1> SocketChat</h1>
          <p> Real-time Chat Application</p>
        </header>

        <Chat></Chat>
      </div>
</template>

<script>
import Chat from './components/Chat.vue'

export default {
  name: 'App',
  components: {
    Chat
  }
}
</script>

<style>
body {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  margin:0px;
  padding:0px;
  list-style: none;
}
ul > li {
	list-style-type: none;
}

header {
    background-color: #3d2aac;
    color: white;
    text-align: left;
    border-radius: 20px 20px 0px 0px;
    height: 100px;
    padding: 20px;
}
</style>
