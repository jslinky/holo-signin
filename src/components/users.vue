<template>
  <section class="c-section-users">
    <template v-for="(user, index) in users">  
        <User :key="user.id" :user="user" :index="index" v-on:update-user-status="updateUserStatus($event)" />    
    </template> 
    <div v-if="users.length == 0">
      <h2 v-if="show == 'out'">Everyone's signed in. What a team!</h2>
      <h2 v-if="show == 'in'">Everyone's signed out. Let's party!!</h2>
    </div>       
  </section>
</template>

<script>

import User from './user'

  export default {
    name:'Users',
    components: {
      User
    },
    props: {
      users: {
        type: Array,
        required: true
      },
      show: {
        validator: function (value) {
          // The value must match one of these strings
          return ['all', 'in', 'out'].indexOf(value) !== -1
        }        
      }      
    },
    methods: {
      updateUserStatus(userId) {
        this.$emit('update-user', userId)
      }
    },
    data() {
      return {

      }
    }
  }
</script>

<style>

.c-section-users {
  perspective: 500px;
}

.c-section-users__person {
  backface-visibility: hidden;
}

.user-enter {
  opacity:0;
}

.user-enter-to {
  opacity:1;
}

.user-enter-active,
.user-leave-active {
  transition: opacity 350ms cubic-bezier(0.165, 0.84, 0.44, 1);
}

.user-leave {
  opacity: 1;
}

.user-leave-to {
  opacity: 0;
}

</style>