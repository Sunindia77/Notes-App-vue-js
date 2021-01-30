<template>
  <div>
    <div>
      <h1>Notes</h1>
    <div>
    <input  v-model="title" type="text" name="title" id="not-title"
    placeholder="Enter title here"><br/>
    <textarea v-model="content" name=" content" id="note-content" cols="40" rows="8"
    placeholder="Type note here"></textarea>
  </div>
  <button @click="submitNote" id="add-note"> Add note</button>
    </div>
    <div>
      <p>Your notes:</p>
      <ul id="notes">
        <li :key="index" v-for="(note, index) in sortedNotes">
          <button @click="deleteNote(note.title)">X</button>
          <h3 class="title-display">{{note.title}]</h3>
          <p>{{note.content}}</p>
        </li>
      </ul>
    </div>
  </div>
</template>

<script>
import db from './firebaseInit'
export default {
  data(){
    return{
      title: '',
      content: '',
      notes: []
    }
  },
  computed:{
      sortedNotes(){
        return this.notes.sort((a, b) =>(a.title> b.title)? 1: -1)
      }
    },
  methods: {
    submitNotes(){
      if(this.title.length>0 && this.content.length>0)
      {
        //add notes to database
        db.collection('notes').add({
          title:this.title,
          content: this.content

        })
        .catch(error => console.log(error))
        //push notes to notes array
        this.notes.push({
          title:this.title,
          content: this.content

        })
        // clear title and content input fields
        this.title= ''
        this.content= ''
      }
    },
    mounted() {
      db.collection('note').get().then(query =>{
        query.forEach(doc=>{
          const dbNote={
            title: doc.data().title,
            content: doc.data().content
          }
          this.notes.push(dbNote)
        })
      })
    },
    
  }
}
</script>
