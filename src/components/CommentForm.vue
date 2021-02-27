<template>
  <form @submit="onSubmit">
    <div class="form-group">
      <label>User</label>
      <input
        v-model="user"
        type="input"
        class="form-control"
        placeholder="fullname"
      />
      <small class="form-text text-muted"
        >le nom de l'utilisateur qui commente</small
      >
    </div>
    <div class="form-group">
      <label>comment</label>
      <textarea
        v-model="comment"
        class="form-control"
        placeholder="comment..."
      />
    </div>
    <br />
    <div class="form-group">
      <input type="submit" class="btn btn-primary" value="Commenter" />
    </div>
  </form>
</template>
<script>
import axios from "axios";
export default {
  name: "CommentForm",
  props: { userid: String },
  data() {
    return {
      user: "",
      comment: "",
    };
  },
  methods: {
    async fetchUserById(id) {
      const res = await axios.get(`http://localhost:5000/users/${id}`);
      const user = await res.data;
      return user;
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
    async onSubmit(e) {
      e.preventDefault();
      var data = {
        user: this.user,
        comment: this.comment,
      };
      console.log(data, this.userid);
     var  userMaj = await this.fetchUserById(this.userid) 
      userMaj.comments.push(data)
      console.log(userMaj)
      axios.put(`http://localhost:5000/users/${this.userid}`, this.formatUserData(userMaj))
      this.$emit('commentAdded', this.userid)
      this.user = "";
      this.comment = "";
      alert("commentaire ajouté")
    },
  },
};
</script>