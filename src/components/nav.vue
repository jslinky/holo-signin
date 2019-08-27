<template>
  <nav role="navigation" class="c-main-nav">
    <h3 class="c-main-nav__title">Show</h3>
    <div class="c-main-nav__links">
      <a href="#" v-for="(link, index) in nav" @click.prevent="setActive(index)" :class="{'active':index == navIndex}">{{link}}</a>
    </div>
  </nav>
</template>

<script>
  export default {
    name:'Nav',
    data() {
      return {
        nav: ['all', 'signed in', 'signed out']
      }
    },
    props: {
      show: {
        validator: function (value) {
          // The value must match one of these strings
          return ['all', 'in', 'out'].indexOf(value) !== -1
        }        
      }       
    },
    computed: {
      navValues() {
        return this.nav.map((entry) => {
          if(entry.includes('signed')) {
            return entry.replace('signed', '').trim()
          }
          return entry
          }        
        )
      },
      navIndex() {
        return this.navValues.indexOf(this.show)
      }
    },
    methods: {
      setActive(i) {
        this.itemIndex = i
        this.$emit('filter-entries', this.navValues[i])
      }
    }
  }
</script>

<style>

.c-main-nav__links a {
    transition: all 70ms ease-in;
}

</style>