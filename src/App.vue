<template>
  <div id="app">
    <div class="header" :class="{'closed': utility == ''}">
      <div class="header__controls">
        <font-awesome-icon icon="search" class='hide' @click="utility = 'search'"></font-awesome-icon>
        <font-awesome-icon class="moving-icon" icon="search" :class="{'active': utility == 'search'}" @click="utility = 'search'"></font-awesome-icon>
        <img class="header__logo" @click="utility = ''" src="@/assets/icons/rocket.png" alt="">
        <img class="header__logo" @click="utility = 'filter'" src="@/assets/icons/filter.png" alt="">
      </div>
      <div class="header__utilities">
        <transition name="modal" mode="out-in">
          <div v-if="utility == 'search'">
            <input  class="header__input" type="text" placeholder="Type a name" v-model="authorNameSearchString" />
            <!-- <font-awesome-icon :class="{'active': utility == 'search'}" icon="search"></font-awesome-icon> -->
          </div>
          <Filters v-if="utility == 'filter'" @clicked="onClickChild"/>
        </transition>
      </div>
    </div>

    <transition-group name="list-animation" tag="div" class="cards-container">
      <Card v-for="user in filteredUserFeed" :user="user" v-bind:key="user.login.uuid" @clicked="openModal"/>
    </transition-group>
    <transition name="modal">
      <Modal v-if="loaded" v-show="modalOpen" :selectedUser="selectedUser" :show="modalOpen" @close="closeModal"/>
    </transition>
  </div>
</template>

<script>
import Card from './components/Card'
import Filters from './components/Filters'
import Modal from './components/Modal'
import axios from 'axios'

export default {
  name: 'App',
	data() { 
    return {
      loaded: false,
      authorNameSearchString: "",
      userFeed: null,
      filter: ['name', 'first'],
      selectedUser: {},
      modalOpen: false,
      utility: 'filter'
    }
  },
  
  components: {
    Card,
    Filters,
    Modal
  },

  methods: {
    onClickChild (value) {
      this.filter = value
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
      this.selectedUser = this.userFeed[0];
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

.hide {
  opacity: 0;
  transition: opacity .5s ease;
  transition-delay: 1s;
}

.moving-icon {
  position: absolute;
  top: 44px;
  transition: top .5s ease;
  z-index: 10;

  &.active {
    top: 97px;
    transition: top .5s ease;
  }
}

.header {
  background-color: #80CBC4;
  position: relative;
  max-height: 500px;
  transition: all 1s;

  &.closed {
    background-color: #EEEEEE;
  }
  
  &__logo {
    padding: 2rem 0 1rem;
  }

  &__utilities{
    max-width: 435px;
    margin: 0 auto;
    padding: 0 15px;

    div {
      position: relative;
    }

    svg {
      position: absolute;
      top: -41px;
      left: 17px;
      font-size: 1.4rem;
      padding-bottom: 20px;
      color: rgb(0, 150, 136);
      transition: top ease .5s;

      &.active {
        top: 11px;
        transition: top 2s ease;
        transition-delay: 2s;
      }
    }
  }

  &__input {
    background: #FCFCFC;
    box-sizing: border-box;
    border: none;
    box-shadow: 0px 0px 2px rgba(0, 0, 0, 0.12);
    border-radius: 2px;
    width: 100%;
    height: 45px;
    font-size: 1rem;
    padding: .5rem .5rem .5rem 3rem;
    margin-bottom: 1rem;
  }

  &::after {
    pointer-events: none;
    position: absolute;
    content: '';
    height: 100px;
    width: 100%;
    bottom: 0;
    transform: translateY(100%);
    left: 0;
    background-image: -webkit-gradient(linear, center top, center bottom, from(rgba(0, 0, 0, 0.25)), to(rgba(77, 77, 77, 0)));
    z-index: 100;
  }

}

.header__controls {
  max-width: 400px;
  margin: 0 auto;
  display: flex;
  justify-content: space-between;
  align-items: baseline;
  padding: 0 30px;

  & * {
    cursor: pointer;
  }
  svg {
    font-size: 1.4rem;
    padding-bottom: 20px;
    color: rgb(0, 150, 136);
  }
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

.modal-enter-active, .modal-leave-active {
  transition: opacity .5s
}
.modal-enter, .modal-leave-to {
  opacity: 0
}
</style>
