<template>
  <article class="c-section-users__person" :id="user.id" :ref="user.id" @dblclick="updateStatus(id)" @click="confirmStatus($event)" :class="{'c-section-users__person--in': user.in, 'c-section-users__person--out': !user.in, 'c-section-users__person--active': confirm, 'confirmed': confirmed}">    
    <div>
      <div class="c-section-users__person__clip"></div>
      <div class="c-person-close-btn">
      </div>
      <div class="c-person-initials">
        <div class="c-person-initials__initials" v-if="!confirmed">
          {{user.fname | initial}}{{user.lname | initial}}
        </div>
        <div class="c-person-initials__signIn">{{user | initialConfirm}} </div>
          <div style="position:absolute; width:100%; left:0; top:0">
          <svg xmlns="http://www.w3.org/2000/svg" height="210" width="210" viewBox='0 0 210 210' v-if="confirmed" class="tick">
            <line id="line1" x1="25" y1="110" x2="85" y2="170" style="stroke:#FEFBE7;stroke-width:15" />
            <line id="line2" x1="75" y1="170" x2="185" y2="60" style="stroke:#FEFBE7;stroke-width:15" />
          </svg>          
          </div>        
      </div>
      <div class="c-person-name">
        <h2>{{user.fname | initial}} {{user.lname}}</h2>
      </div>
      <div class="c-person-status-warning">
        <!-- Are you sure message -->
        Are you sure?
      </div>
    </div>    
  </article>
</template>

<script>
  export default {
    name:'User',
    props: {
      user: {
        required: true
      },
      index: {
        required:true
      }
    },
    data() {
      return {
        confirm: false,
        confirmed: false,
        signInTxt: 'Sign'
      }
    },
    methods: {
      confirmStatus(e) {
        const confirmTarget = !Array.from(e.target.classList).includes('c-person-close-btn')
        if(this.confirm && confirmTarget) {
          this.updateStatus(this.user.id)
        }        
        this.confirm = (confirmTarget ? true : false)               
      },
      updateStatus(userId) {
        this.confirmed = true
        const delay = time => new Promise(resolve => setTimeout(resolve, time))
        delay(1000).then(() => {
          this.$emit('update-user-status', userId)
          this.confirmed = false
          this.confirm = false
        })        
      }
    },
    computed: {
      status() {
        return this.user.in
      }
    },
    filters: {
      initialConfirm: function(value) {        
        return value.in ? 'Sign out' : 'Sign in'
      },
      initial: function (value) {
        if (!value) return ''
        value = value.toString()
        return value.charAt(0)
      }      
    }
  }
</script>

<style>

line {
  animation: line 150ms linear forwards;
  stroke:#395E2F;
}
#line2 {
animation-delay:150ms
}
#line1 {
  stroke-dasharray:85;
  stroke-dashoffset:85;
}
#line2 {
  stroke-dasharray:170;
  stroke-dashoffset:170;
}
@keyframes line {
  to {
  stroke-dashoffset:0;    
  }
}

.c-person-initials {
  position: relative;
}

.tick {
  width: 100%;
  height: 100%;
}

.c-section-users__person--active.confirmed .c-person-status-warning {
  transform: translateY(100%)!important;
}


.user-enter {
  transform:rotateY(90deg);
}

.user-enter-to {
  transform:rotateY(0);
}

.user-enter-active,
.user-leave-active {
  transition: transform 1350ms cubic-bezier(0.165, 0.84, 0.44, 1);
}

.user-leave {
  transform:rotateY(-90deg);
}

</style>