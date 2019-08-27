<template>
  <div id="app" class="app">
    <Header :user="statusUser" />
    <Nav v-on:filter-entries="filterEntries($event)" :show="filterUsers" />
    <main class="c-section-main-container">
      <Users :users="userGroup" :show="filterUsers" v-on:update-user="updateUserStatus($event)" />
      <aside class="c-section-status-container">
        <Status :users="signedIn" show="in" />
        <Status :users="signedOut" show="out" />
        <!-- <Status :users="users" /> -->
      </aside>
    </main>
    <Footer />
  </div>
</template>

<script>
import Header from './components/header.vue'
import Nav from './components/nav.vue'
import Users from './components/users.vue'
import Status from './components/status.vue'
import Footer from './components/footer.vue'

export default {
  name: 'app',
  components: {
    Header,
    Nav,
    Users,
    Status,
    Footer
  },
  data() {
    return {
      users:[
        'Freja Brook',
        'Elmer Bautista',
        'Taio Parkes',
        'Aston Wallis',
        'Sarah-Jane Sellers',
        'Pierre Haines',
        'Ivie Buxton',
        'Esha Ramirez',
        'Cara Wells',
        'Taylor Stubbs',
        'Tallulah Friedman',
        'Leilani Draper',
        'Kylie Martin',
        'Abbigail Needham',
        'Eshaan Cairns',
        'Nicole Guzman',
        'Effie Shea',
        'Miley Sanderson',               
      ],      
      filterUsers: 'all',
      statusUser: {
        fname: 'Joe',
        in: false        
      }
    }    
  },
  methods: {
    userSetup() {
      return this.users.map(user => user.split(' ')).reduce((acc = [], currentVal, i) => {
        let person = {}
        person.fname = currentVal[0]
        person.lname = currentVal[1]
        person.initials = `${currentVal[0].charAt(0)} ${currentVal[1].charAt(0)}` 
        if(i % 2 == 0) {
          person.in = false
        } else {
          person.in = true
        }
        person.id = person.fname.toLowerCase() + '-' + i
        person.activity = []
        person.activity.push(this.getTimeStatus(person.in ? 'in' : 'out', true))        
        acc.push(person)
        return acc
      }, [])
    },
    getTimeStatus(status, setup) {

      function getRandomInt(max) {
        let num = Math.floor(Math.random() * Math.floor(max))
        if(num < 10) {
          num = ('0' + num)
        }
        return num
        // return Math.floor(Math.random() * Math.floor(max));
      }
      let currentHr = new Date().getHours(),
          randomHr = getRandomInt(currentHr),
          date = new Date()

      // set random hr if on setup
      if(setup) {
        date.setHours(randomHr)
      }
      
      return {
        date,
        status
      }
    },
    filterEntries(e) {
      this.filterUsers = e
      // set scrollTop value on container
      // Set local storage
      const parsed = JSON.stringify(this.filterUsers);
      localStorage.setItem('filterUsers', parsed);      
    },
    updateUserStatus(userId) {
      for(let user of this.users) {
        if(user.id == userId) {
          user.in = !user.in
          user.activity.push(this.getTimeStatus(user.in, false))
          console.log(user.in)
          this.$set(this.statusUser, "fname", user.fname)
          this.$set(this.statusUser, "in", user.in)          
        }        
      }
      // Set local storage
      const parsed = JSON.stringify(this.users);
      localStorage.setItem('users', parsed);
    },
    getLocal(key = '') {
      if (localStorage.getItem(key)) {
        try {
          this[key] = JSON.parse(localStorage.getItem(key));
        } catch(e) {
          localStorage.removeItem(key);
        }
      } else {
        this[key] = this.userSetup()
        localStorage.setItem(key, JSON.stringify(this[key]))
      }      
    }
  },
  computed: {
    userGroup() {
      if(this.filterUsers == 'all') {
        return this.allUsers
      } else if(this.filterUsers == 'in') {
        return this.signedIn
      } else if(this.filterUsers == 'out') {
        return this.signedOut
      }
    },
    allUsers() { return this.users },
    signedOut() { return this.users.filter(user => !user.in) },
    signedIn() { return this.users.filter(user => user.in) }
  },  
  mounted() {  
    this.getLocal('users')
    this.getLocal('filterUsers')

    // if (localStorage.getItem('users')) {
    //   try {
    //     this.users = JSON.parse(localStorage.getItem('users'));
    //   } catch(e) {
    //     localStorage.removeItem('users');
    //   }
    // } else {
    //   this.users = this.userSetup()
    //   localStorage.setItem('users', JSON.stringify(this.users))
    // }

  }   
}
</script>

