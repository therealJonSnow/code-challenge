<template>
  <div @click="open = !open;" class="card" :class="{ 'card--expanded': open }">
    <div class="card-overview">
      <img class="card-overview__image" :src="user.picture.large" alt="">
      <svg class="card-overview__phone" v-on:click.stop="callModal" width="48" height="48" viewBox="0 0 48 48" fill="none" xmlns="http://www.w3.org/2000/svg">
        <circle cx="24" cy="24" r="24" fill="#009688"/>
        <path opacity="0.54" fill-rule="evenodd" clip-rule="evenodd" d="M15 15C14.447 15 14 15.447 14 16C14 25.388 21.612 33 31 33C31.553 33 32 32.553 32 32V28.5C32 27.948 31.553 27.501 31 27.501C29.752 27.501 28.552 27.3 27.428 26.932C27.328 26.9 27.223 26.884 27.119 26.884C26.864 26.884 26.608 26.982 26.413 27.177L24.212 29.38C21.38 27.94 19.065 25.625 17.623 22.794L19.823 20.587C20.098 20.313 20.179 19.918 20.068 19.572C19.7 18.447 19.499 17.247 19.499 16C19.499 15.447 19.052 15 18.5 15H15Z" fill="white"/>
      </svg>

      <div class="card-overview__inner">
        <p class="card-overview__name">{{ user.name.first + ' ' + user.name.last}}</p>
        <p class="card-overview__location">{{ user.location.state + ', ' + user.location.country}}</p>
      </div>
    </div>
    <div class="card-details">
      <div class="card-details__section">
        <img src="@/assets/icons/ghost.png" alt="">
        <div>
          <p>{{ user.email }}</p>
          <p>{{ user.cell }}</p>
          <p>{{ user.phone }}</p>
        </div>
      </div>
      <div class="card-details__section">
          <img src="@/assets/icons/building.png" alt="">
          <div>
            <p>{{ user.location.street.number + " " + user.location.street.number }}</p>
            <p>{{ user.location.city }}</p>
            <p>{{ user.location.state }}</p>
            <p>{{ user.location.country }}</p>
          </div>
      </div>
    </div>
    <font-awesome-icon class="card__chevron" :class="{ active: open }" icon="chevron-down" ></font-awesome-icon>
  </div>
</template>

<script>
/*jshint esversion: 6 */
export default {
  name: 'Card',

  props: {
    user: Object
  },

  data: function() {
    return {
      plus: true,
      open: false,
    }
  },

  methods: {
    callModal: function() {
      const vm = this;
      this.$emit('clicked', vm.user);
    }
  }
}
</script>

<style lang="scss">

.card {
  background: #FAFAFA;
  border-radius: 3px;
  box-shadow: 0px 0px 2px rgba(0, 0, 0, 0.12), 0px 2px 2px rgba(0, 0, 0, 0.24);
  color: #616161;
  cursor: pointer;
  margin-bottom: .5rem;
  margin: 0 auto 7px;
  max-width: 344px;
  position: relative;
  position: relative;
  text-align: left;
  transition: all 1s;

  &__chevron {
    bottom: 10px;
    font-size: 0.7rem;
    left: auto;
    position: absolute;
    right: 10px;
    top: auto;
    transition: transform 1s ease;
      
    &.active {
        transform: rotate(180deg);
        transition: transform 1s ease;
    }
  }

  &--expanded {
    .card-overview {
      height: 12rem;
      transition: height 1s ease;

      &__image {
        filter: brightness(0.6);
        transition: all 1s ease;
        width: 100%;
      }
      &__inner {
        color: white;
        padding: 7rem 1rem 2rem 30%;
        transition: all 1s ease;
      }
    }
    .card-details {
      max-height: 500px;
      transition: max-height 1s ease;
    }
  }
}
.card-overview {
  align-items: center;
  display: flex;
  height: 5.5rem;
  overflow: hidden;
  position: relative;
  transition: height 1s ease;

  &__image {
    position: absolute;
    transform-origin: top left;
    transition: all 1s ease;
    width: 26%;
  }

  &__phone {
    left: 27px;
    position: absolute;
    top: 114px;
    transition: opacity .5s ease;
    transition: transform .5s ease;
    z-index: 10;

    path {
      transition: opacity .5s ease;
    }

    &:hover {
      transform-origin: center;
      transform: scale(1.05);
      transition: transform .5s ease;
      
      path {
        opacity: 1;
        transition: opacity .5s ease;
      }
    }
  }

  &__inner {
    display: flex;
    flex-basis: 100%;
    flex-direction: column;
    padding: 2rem 1rem 2rem 30%;
    transition: all 1s ease;
    z-index: 5;
  }
  &__name {
    font-size: 1.31rem;
    font-weight: bold;
    margin: 0;
  }

  &__location {
    font-size: 14px;
    font-weight: normal;
    margin: 0;
    opacity: 0.8;
  }
}

.card-details {
  font-size: .9rem;
  max-height: 0;
  overflow-y: hidden;
  position: relative;
  transition: max-height 1s ease;

  &__section {
    align-items: flex-start;
    display: flex;
    justify-content: space-between;
    margin: 1.25rem;

    div {
      flex-basis: 75%;
      padding-left: 1rem;
      position: relative;

      p {
        font-size: 14px;
        line-height: 21px;
        margin: 0;
      }

      &::before {
        background-color: gray;
        content: '';
        height: 100%;
        left: 0;
        position: absolute;
        top: 0;
        width: 1px;
      }
    }

    img {
        padding: 0 1.2rem;
    }
  }
}
</style>

