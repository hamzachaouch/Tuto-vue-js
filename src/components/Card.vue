<template>
 
    <div class="card shadow mb-4" :id="user.gender">
          <div class="card-body row">
            <img
              :src="user.pic"
              class="float-left rounded-circle col-md-4"
            />
            <div class="message col-md-8">
              <h5 class="card-title">{{user.fullname}}</h5>
              <h6 class="card-subtitle mb-2 text-muted">{{user.fullname}}</h6>
            </div>
            <div class="message col-md-12">
              <p class="card-text">
                    {{user.description}}
               </p>
            </div>
            <div class="actions">
               
              <a href="#" v-if="$router.currentRoute._rawValue.name!=='Friends'" @click="$emit('likeAction', user.id)" class="card-link"><i :class="user.liked ? 'fas fa-thumbs-up':'far fa-thumbs-up'"></i></a>
              <a href="#"  v-if="$router.currentRoute._rawValue.name=='Friends'" class="card-link"><i class="fas fa-comments"></i></a>
              <a href="#"  v-if="$router.currentRoute._rawValue.name!=='Friends'" @click="$emit('friendsAction', user.id)" class="card-link"><i :class="user.friends ? 'fas fa-user-times friend':'fas fa-user-plus'"></i></a>
            </div>
          </div>
        </div>
</template>
<script>
export default {
    name: 'Card',
    props: {
        user: Object
    },
    data() {
      
       return{
           path: this.$router.currentRoute
       }
    }
}
</script>