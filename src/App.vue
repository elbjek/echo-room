<template>
  <div id="app">
      <h1>Echo room</h1>

      <p v-for="msg in messages" :key="msg.id">{{ msg }}</p>

      <form action="#" method="post" @submit.prevent="sendMsg">
        <input type="text" v-model="newMsg" autofocus placeholder="Send a message">
        <button>SEND</button>
      </form>

        <button @click="connect">Connect</button>
        <button @click="disconnect">Disconnect</button>

  </div>
</template>

<script>


export default {
  name: 'app',
  data(){
    return{
      newMsg:"",
      messages:[],
      socket:""
    }
  },
  mounted(){

   this.$nextTick(function () {
      this.connect();
    })

  },
  methods:{

    sendMsg(){

      this.socket.send(this.newMsg);
      this.messages.push('Sent message: '  + this.newMsg );
      this.newMsg="";

      return false;

    },
    connect(){
      this.socket = new WebSocket('wss://echo.websocket.org/');

      this.socket.addEventListener('open',()=>{
      this.messages.push("Connected to server");
    })

      this.socket.addEventListener('message',(e)=>{
        this.messages.push('Returned message: '   + e.data);
      })

    },
    disconnect(){
      this.socket.close();
      console.log('Disconnected from the server');
      this.messages.push("Disconnected from the server");
    }
  }
}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  text-align: center;
  background-color: gray;
  width:700px;
  margin:0 auto;
  border-radius: 8px;
}
h1{
  background-color: orangered;
  color:white;
  font-weight: 900;
}
#app > p{
  color:white;
  font-weight: bold;
}

form>input{
  width:250px;
  margin:0 auto;
  padding: 8px 10px;
  margin-bottom:30px
}
form>button{
  color:black;
  border:none;
  border-radius: 3px;
  margin-left: 10px;
  margin-right: 0;
  background-color: white;
}
button{
  border:none;
  color:white;
  background-color: orangered;
  font-weight: 700;
  padding:8px 15px;
  border-radius: 8px;
  margin: 0px 20px 30px 20px
}

</style>
