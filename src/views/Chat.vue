<template>
  <v-card max-width="500" class="mx-auto">
    <v-card-text>
      <h1 class="text-center mt-5 pb-10 indigo--text">Ninja Chat</h1>
      <div class="message--container" v-chat-scroll>
        <v-layout justify-center>
          <v-expansion-panels accordion>
            <v-expansion-panel v-for="message in messages" :key="message.id">
              <v-expansion-panel-header disable-icon-rotate>
                <span><b class="indigo--text">{{ message.name }}</b>: {{ message.content }}</span> 
                <template v-slot:actions>
                  <v-icon color="teal">done</v-icon>
                </template>
              </v-expansion-panel-header>
              <v-expansion-panel-content class="grey--text">
                Send: {{ message.timestamp }}
              </v-expansion-panel-content>
            </v-expansion-panel>
          </v-expansion-panels>
        </v-layout>
      </div>
      <div class="send-container">
      <NewMessage :person="person" />
      </div>
    </v-card-text>
  </v-card>
</template>

<script>
import NewMessage from "@/components/NewMessage";
import db from '@/firebase/init'
import moment from 'moment'

export default {
  name: 'Chat',
  props: ["person"],
  components: {
    NewMessage,
  },
  data(){
    return {
      messages: []
    }
  },
  created(){
    db.collection('messages').orderBy('timestamp').onSnapshot(snapshot => {
      snapshot.docChanges().forEach(change => {
        if(change.type == 'added'){
          this.messages.push({
            id: change.doc.id,
            name: change.doc.data().name,
            content: change.doc.data().content,
            timestamp: moment(change.doc.data().timestamp).format('LLLL')
          });
        }
      })
    })
  }
};
</script>

<style scoped>
.message--container {
  max-height: 600px;
  overflow-x: hidden;
  overflow-y: scroll;
}
.message--container::-webkit-scrollbar {
  width: 4px;
}
.message--container::-webkit-scrollbar-track {
  background: #f1f1f1; 
}
.message--container::-webkit-scrollbar-thumb {
  background: rgb(197, 197, 197); 
}

.send-container{
  height: 80px;
  overflow: hidden;
}
</style>