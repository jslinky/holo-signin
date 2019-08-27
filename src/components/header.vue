<template>
  <header role="heading" class="c-main-header">
    <div class="c-main-header__status-msg" :class="{'active': showStatus}">
      <h1 class="inverted" v-if="user.in">Welcome {{user.fname}}. Thanks for signing in.</h1>
      <h1 class="inverted" v-else-if="!user.in">Thanks for signing out {{user.fname}}. Come back soon!</h1>
    </div>
    <div class="c-main-header__msg">
      <h1 class="inverted">Please update your status when entering or leaving the building</h1>
    </div>
  </header>
</template>

<script>
  export default {
    name:'Header',
    props: {
      user: {
        type: Object      
      }
    },
    data() {
      return {
        showStatus:false
      }
    },
    computed: {
      userComp() {
        return this.user
      }
    },
    methods: {
      setStatus(duration) {
        setTimeout(() => {
          this.showStatus = false
        }, duration)        
      }
    },
    watch: {
      userComp: {
        handler(val){
          this.showStatus = true
          this.setStatus(2000)
        },
        deep: true
      }
    }   
  }
</script>
