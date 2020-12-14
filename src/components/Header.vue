<template>
  <div class="header" :class="{'closed': utility == ''}">

    <div class="header-controls">
      <!-- hidden to hold space for abs positioned icon -->
      <font-awesome-icon icon="search" class='header-controls__search' @click="utility = 'search'"></font-awesome-icon>
      <font-awesome-icon class="header-controls__search header-controls__search--anim" icon="search" :class="{'active': utility == 'search'}" @click="utility = 'search'"></font-awesome-icon>
      <img class="header-controls__logo" @click="utility = ''" src="@/assets/icons/rocket.png" alt="">
      <img class="header-controls__filter" :class="{'active': utility == 'filter'}" @click="utility = 'filter'" src="@/assets/icons/filter.png" alt="">
    </div>

    <div class="header-utilities">
      <transition name="fader" mode="out-in">
        <div v-if="utility == 'search'">
          <input @keyup="searchUpdate" class="header-utilities__input" type="text" placeholder="Type a name" v-model="authorNameSearchString" />
        </div>
        <Filters v-if="utility == 'filter'" @clicked="onClickChild"/>
      </transition>
    </div>
  </div>
</template>

<script>

import Filters from './Filters'
export default {
  name: 'Header',
  components: {
    Filters
  },
	data() { 
    return {
      loaded: false,
      authorNameSearchString: "",
      utility: '',
    }
  },
  

  methods: {
    onClickChild (value) {
      this.$emit('filterChange', value);
    },
    searchUpdate: function() {
      this.$emit('searchUpdate', this.authorNameSearchString);
    }
  }
}
</script>

<style lang="scss">

.header {
  background-color: #80CBC4;
  max-height: 500px;
  position: relative;
  transition: all 1s;

  &.closed {
    background-color: #EEEEEE;
  }
  
  &::after {
    background-image: -webkit-gradient(linear, center top, center bottom, from(rgba(0, 0, 0, 0.25)), to(rgba(77, 77, 77, 0)));
    bottom: 0;
    content: '';
    height: 100px;
    left: 0;
    pointer-events: none;
    position: absolute;
    transform: translateY(100%);
    width: 100%;
    z-index: 100;
  }
}

.header-utilities{
  margin: 0 auto;
  max-width: 435px;
  padding: 0 15px;

  svg {
    color: rgb(0, 150, 136);
    font-size: 1.4rem;
    left: 17px;
    padding-bottom: 20px;
    position: absolute;
    top: -41px;
    transition: top ease .5s;

    &.active {
      top: 11px;
      transition-delay: 2s;
      transition: top 2s ease;
    }
  }
  
  &__input {
    background: #FCFCFC;
    border-radius: 2px;
    border: none;
    box-shadow: 0px 0px 2px rgba(0, 0, 0, 0.12);
    box-sizing: border-box;
    font-size: 1rem;
    height: 45px;
    margin-bottom: 1rem;
    padding: .5rem .5rem .5rem 3rem;
    width: 100%;

    &:focus {
      outline: 2px solid #ffa01b;
    }
  }
}

.header-controls {
  align-items: baseline;
  display: flex;
  justify-content: space-between;
  margin: 0 auto;
  max-width: 400px;
  padding: 0 30px;

  & * {
    cursor: pointer;
  }

  &__search {
    color: rgb(0, 150, 136);
    font-size: 1.4rem;
    opacity: 0;
    padding-bottom: 16px;

    &--anim {
      opacity: 1;
      position: absolute;
      top: 48px;
      transition: top .5s ease;
      z-index: 10;

      &.active {
        top: 97px;
        transition: top .5s ease;
      }
    }
  }
  &__filter{
    padding: 2rem 0 1rem;
    &.active{
      filter: brightness(10);
    }
  }
  &__logo {
    padding: 2rem 0 1rem;
  }
}
</style>