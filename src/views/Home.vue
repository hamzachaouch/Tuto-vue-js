<template>
  <div class="home">
    <Suggestions :users="users" />
  </div>
</template>

<script>
// @ is an alias to /src
import Suggestions from '@/components/Suggestions.vue'

export default {
  name: 'Home',
  components: {
    Suggestions
  }, 
  emits:['likeAction', 'friendsAction'],
  props:{
    users:Array
  }
}
</script>
