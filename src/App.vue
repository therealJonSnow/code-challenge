<template>
  <div id="app">
    <div class="header">
      <div class="header__controls">
        <font-awesome-icon icon="search"></font-awesome-icon>
        <img class="header__logo" src="@/assets/icons/rocket.png" alt="">
        <img class="header__logo" src="@/assets/icons/filter.png" alt="">
      </div>
      
      
      <div class="header__utilities">
        <input class="header__input" type="text" placeholder="Type a name" v-model="authorNameSearchString" />
        <Filters @clicked="onClickChild"/>
      
      </div>
    </div>
    <transition-group name="list-animation" tag="div" class="cards-container">
      <Card v-for="user in filteredUserFeed" :user="user" v-bind:key="user.login.uuid"/>
    </transition-group>
  </div>
</template>

<script>
import Card from './components/Card'
import Filters from './components/Filters'
import axios from 'axios'

export default {
  name: 'App',
	data() { 
    return {
		authorNameSearchString: "",
    userFeed: null,
    filter: ['name', 'first']
    }
	},
  components: {
    Card,
    Filters
  },

  methods: {
    onClickChild (value) {
      this.filter = value
    }
  },

  computed: {
    filteredUserFeed: function () {
      const vm = this;
      var users = this.userFeed;
      // Check if there is a search term, if so implement
      var authorNameSearchString = this.authorNameSearchString;
      if(authorNameSearchString){
        let searchString = authorNameSearchString.trim().toLowerCase();
        users = users.filter(function(item){
          if(item.name.first.toLowerCase().indexOf(searchString) == 0){
            return item;
          } 
          if (item.name.last.toLowerCase().indexOf(searchString) == 0) {
            return item;
          }
          if (item.location.state.toLowerCase().indexOf(searchString) == 0) {
            return item;
          }
          if (item.location.country.toLowerCase().indexOf(searchString) == 0) {
            return item;
          }
        })
      }

      //Now filter those results
      if(users) {
        console.log(users[0]);
        let filter1 = vm.filter[0];
        let filter2 = vm.filter[1];
        console.log(filter1, filter2)
        users = users.sort(function(a, b){
          if(a[filter1][filter2] < b[filter1][filter2]) { return -1; }
          if(a[filter1][filter2] > b[filter1][filter2]) { return 1; }
          return 0;
        });
      }
      return users;
    },
  },
  mounted() {
  axios
    .get('https://www.randomuser.me/api/?results=100')
    .then(response => {
      this.userFeed = response.data.results.sort(function(a, b){
        if(a.name.first < b.name.first) { return -1; }
        if(a.name.first > b.name.first) { return 1; }
        return 0;
      })
    })
    .catch(error => console.log(error))
  }

}
</script>

<style lang="scss">

body {
  margin: 0;
  height: 100vh;
  width: 100vw;
  overflow: hidden;
  box-sizing: border-box;
  background-color: #7b797929;
}
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  
  position: relative;
}

#app div{
  position: relative;
}

.header {
  background-color: #80CBC4;
  margin-bottom: 2rem;


  
  &__logo {
    padding: 2rem 0 1rem;
  }

  &__utilities{
    width: 100%;
    max-width: 400px;
    margin: 0 auto;
  }

  &__input {
    width: 90%;
    height: 1.5rem;
    border: none;
    border-radius: 3px;
    font-size: 1rem;
    padding: .5rem;
    margin-bottom: 1rem;
  }

}

.header__controls {
  max-width: 400px;
  margin: 0 auto;
  display: flex;
  justify-content: space-between;
  align-items: baseline;
  padding: 0 30px;

  svg {
    font-size: 1.4rem;
    padding-bottom: 20px;
  }
}


.cards-container {
  max-height: 50vh;
  overflow: scroll;
}

.list-animation-enter,
.list-animation-leave-to {
  opacity: 0;
  max-height: 0px;
  padding-top: 0px !important;
  padding-bottom: 0px !important;
  overflow: hidden;
}
.list-animation-enter-to,
.list-animation-leave {
  opacity: 0;
  max-height: 80px;
}

</style>
