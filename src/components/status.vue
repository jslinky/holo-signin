<template>
  <section class="c-section-status" :class="{'c-section-status--out': show == 'out', 'c-section-status--in': show == 'in'}">
    <header>
      <h3 :data-number="users.length" v-if="show == 'out'">Signed out</h3>
      <h3 :data-number="users.length" v-if="show == 'in'">Signed in</h3>
      <h4>Most Recent</h4>
    </header>    
    <div class="c-section-status__list">
      <template v-for="(entry, index) in mostRecent">
        <div :key="entry.id" :class="{'active': usersAdded && index == 0}">{{entry.fname}} {{entry.lname}}
          <time>{{entry.activity[entry.activity.length - 1].date | formatDate}}</time>
        </div>
      </template>
    </div>    
  </section>
</template>

<script>
  export default {
    name:'Status',
    data() {
      return {
        addedActive: false,
        usersAdded: false,
        userCount: 0
      }
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
    computed: {
      dates() {
        return this.users.sort((a, b) => {
          let date1 = Date.parse(a.activity[(a.activity.length - 1)].date),
              date2 = Date.parse(b.activity[(b.activity.length - 1)].date)                   
          if (date1 < date2) return 1;
          if (date1 > date2) return -1;
          return 0;          
        })      
      },
      mostRecent() {
        return this.dates.filter((entry, index) => {
          if(index <= 2) {
            return entry
          }
        })
      },
      userCounter() {
        return this.users.length
      }
    },
    methods: {
      removeClass() {
        setTimeout(() => {  
          this.usersAdded = false
          // this.addedActive = false
        }, 800)        
      }
    },
    filters: {
      formatDate: function (value) {
        let date = new Date(value),
            hrs = date.getHours(),
            minutes = date.getMinutes()
            hrs < 10 ? hrs = '0' + hrs : hrs
            minutes < 10 ? minutes = '0' + minutes : minutes
            if(hrs > 12 && hrs < 24) {
              hrs = hrs - 12
              return `${hrs}.${minutes} pm` 
            } else {
              return `${hrs}.${minutes} am` 
            }
        // return `${hrs}.${minutes}` 
      }
    },
    watch: {
      userCounter: function (newVal, oldVal) {        
        if(newVal > oldVal) {
          this.usersAdded = true
        } else {
          this.usersAdded = false
          this.addedActive = false
        }
      },
      dates: function (newVal, oldVal) {
        if(newVal.length > 0 && oldVal.length > 0) {
          if(newVal[0].id !== oldVal[0].id && this.usersAdded) {
            this.addedActive = true
            this.removeClass()
          }          
        }        
      }      
    }
  }
</script>

<style scoped>

.c-section-status {
  display: block!important;
}

@keyframes slide {
  3% { width:0.375rem; }
  50% { width:100%; }
  100% { width:0.375rem; }
}
@keyframes slide-text {
  0% { color:transparent; }
  90% { color: inherit; }
}


.c-section-status__list > div:first-child.active {
  animation: slideText 250ms ease-in forwards;
}

.c-section-status__list > div:first-child.active:before {
  animation: slide 250ms ease-in forwards;
}

.c-section-status h3:after {
    width: 1.5em;
    height: 1.5em;
    text-align: center;
}




</style>