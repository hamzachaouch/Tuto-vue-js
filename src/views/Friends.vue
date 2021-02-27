<template>
  <div class="about">
  <h1>This is the Friends page</h1>
    <div class="container">
      <div class="row">
        <div :key="user.id" class="col-lg-3" v-for="user in users">
          <div v-show="user.friends">
              <Card @click="displayComments(user.id)" :user="user" />
          </div>
        </div>
      </div>
      <hr class="featurette-divider">
      <div>
        <Comments @commentAdded="commentAdded" :userid = "id" :comments="comments" />
      </div>
    </div>
  
  </div>
</template>
<script>
import Card from '@/components/Card.vue'
import Comments from '@/components/Comments.vue'
import axios from 'axios'
export default {
  name: 'Friends',
  components:{
    Card,
    Comments
  },
  data(){
    return {
      users: [],
      comments:[],
      id:null
    }
  },
 async mounted(){
  var data = await this.fetchUsers();
   
   this.users = data.filter(item=>{
     return item.friends ==true
   })
  },
  methods:{
  async  commentAdded(userid){
          // this.$emit('commentAdded', userid)
          // console.log('friends',userid)
          this.comments = await this.fetchUserById(userid).Comments
          console.log(this.comments, 'this.comments')
        },
    async fetchUserById(id){
      const res = await axios.get(`http://localhost:5000/users/${id}`)
      const user = await res.data;
      return user;
      },
    async displayComments(id){
      this.id=id;
      var data = await this.fetchUserById(id);
      this.comments=data.comments;     
    },
     async fetchUsers(){
    const res =await axios.get('http://localhost:5000/users');
    const data =await res.data;
    return data;
     },
  }
  
}
</script>