<template>
  <div class="about">
    <h1>This is the Home page</h1>
    <div class="container">
      <div class="row">
        <div :key="user.id" class="col-lg-3" v-for="user in users">
          <card
            @likeAction="toggleLikeAction"
            @friendsAction="toggleFriendsAction"
            :user="user"
          />
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import Card from "./Card.vue";
import axios from "axios";
export default {
  components: { Card },
  name: "Suggestions",
  data(){
    return {
      users : []
    }
  },
   mounted: async function(){
  this.users = await  this.fetchUsers();




  },
  methods:{
    async fetchUserById(id){
      const res = await axios.get(`http://localhost:5000/users/${id}`)
      const user = await res.data;
      return user;
      },
    async fetchUsers(){
    const res =await axios.get('http://localhost:5000/users');
    const data =await res.data;
    return data;
     },
     formatUserData(user){
       const {id,fullname,pic,gender,description,friends,liked,comments} = user
       return {    
        "id": id,
        "fullname": fullname,
        "pic": pic,
        "gender":gender,
        "description": description,
        "friends": friends,
        "liked": liked,
        "comments":comments
      }
     },
   async updateUserById(user){
     const res = await axios.put(`http://localhost:5000/users/${user.id}`,
       this.formatUserData(user)
     );
    const data= await res.data
    return data
   } , 
   async toggleFriendsAction(id){
       var userToUpdate = await this.fetchUserById(id);
       
       var user = {...userToUpdate,friends : !userToUpdate.friends}
       await this.updateUserById(user) 
        this.users.map(item =>{
          if(item.id == id){
          
             item.friends = user.friends
            
          }
             return item 
          
        })
   
    },
  async  toggleLikeAction(id){
    //  console.log(id)
       var userToUpdate = await this.fetchUserById(id);
       var user = {...userToUpdate,liked : !userToUpdate.liked}
       await this.updateUserById(user) 
    this.users.map(item =>{
          if(item.id == id){
          
             item.liked = user.liked
            
          }
             return item 
          
        })
    
    
    }
  }
  
 
};
</script>

