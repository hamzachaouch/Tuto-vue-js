<template>
    <h1>commentaires</h1>
    
    <div :key="c.id" v-for="c in comments">
        <h5>{{c.user}}</h5>
        <p>{{c.comment}}</p>
    </div>
    <div v-if="userid != null"> <CommentForm @commentAdded="commentAdded" :userid = "userid"/></div>
    <div v-else><p>Veillez séléctionner une personne</p></div>
</template>
<script>
import CommentForm from './CommentForm'
import axios from "axios";

export default {
    name : "Comments",
    props:{
        comments: Array,
        userid: String
    },
    data(){
        return {
          commentData : []      
        }
    },
    components:{
        CommentForm
    },
     emits: ["commentAdded"],
    methods:{
        async fetchUserById(id){
      const res = await axios.get(`http://localhost:5000/users/${id}`)
      const user = await res.data;
      return user;
      },
     commentAdded(userid){
           this.$emit('commentAdded', userid)
         
          
        }
    }

}
</script>