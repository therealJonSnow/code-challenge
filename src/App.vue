<template>
  <div id="app">
    <Header @searchUpdate="searchUpdate" @filterChange="filterChange"></Header>
    <transition-group name="list-animation" tag="div" class="cards-container">
      <Card v-for="user in filteredUserFeed" :user="user" v-bind:key="user.login.uuid" @clicked="openModal"/>
    </transition-group>
    <transition name="fader">
      <Modal v-if="loaded" v-show="modalOpen" :selectedUser="selectedUser" :show="modalOpen" @close="closeModal"/>
    </transition>
  </div>
</template>

<script>
import Header from './components/Header'
import Card from './components/Card'
import Modal from './components/Modal'
import axios from 'axios'

export default {
  name: 'App',

  components: {
    Card,
    Modal,
    Header
  },

	data() { 
    return {
      loaded: false,
      authorNameSearchString: "",
      userFeed: null,
      filter: ['name', 'first'],
      selectedUser: {},
      modalOpen: false
    }
  },

  methods: {
    filterChange(value) {
      this.filter = value;
    },
    searchUpdate (searchValue) {
      this.authorNameSearchString = searchValue
    },
    openModal (user) {
      this.selectedUser = user;
      this.modalOpen = true;
    },
    closeModal() {
      this.modalOpen = false;
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

      //Now filter those results based on filter selected
      if(users) {
        let filter1 = vm.filter[0];
        let filter2 = vm.filter[1];
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
        //Sort response alphabetically for initial display
        this.userFeed = response.data.results.sort(function(a, b){
          if(a.name.first < b.name.first) { return -1; }
          if(a.name.first > b.name.first) { return 1; }
          return 0;
        })
        //Pass base data to modal component so doesn't error
        this.selectedUser = this.userFeed[0];
        //Render modal (although still hidden)
        this.loaded = true;
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

  & > * {
    box-sizing: border-box;
  }
}
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  height: 100vh;
  position: relative;

  display: flex;
  flex-direction: column;
}


.cards-container {
  overflow-y: scroll;
  padding: 20px 0;
  position: relative;

  &::after {
    pointer-events: none;
    position: fixed;
    content: '';
    height: 100px;
    width: 100%;
    bottom: 0;
    left: 0;
    background-image: -webkit-gradient(linear, center bottom, center top, from(rgba(0, 0, 0, 0.25)), to(rgba(77, 77, 77, 0)));
    z-index: 100;
  }
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

.fader-enter-active, .fader-leave-active {
  transition: opacity .5s
}
.fader-enter, .fader-leave-to {
  opacity: 0
}
</style>
