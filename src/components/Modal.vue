<template>
    <div class="modal"  :style="'background-image: url(' + selectedUser.picture.large + ');'">
      <div class="modal-inner">
        <svg v-on:click.stop="closeModal" width="48" height="48" viewBox="0 0 48 48" fill="none" xmlns="http://www.w3.org/2000/svg">
            <circle cx="24" cy="24" r="24" fill="#DB4437"/>
            <path opacity="1" fill-rule="evenodd" clip-rule="evenodd" d="M15 15C14.447 15 14 15.447 14 16C14 25.388 21.612 33 31 33C31.553 33 32 32.553 32 32V28.5C32 27.948 31.553 27.501 31 27.501C29.752 27.501 28.552 27.3 27.428 26.932C27.328 26.9 27.223 26.884 27.119 26.884C26.864 26.884 26.608 26.982 26.413 27.177L24.212 29.38C21.38 27.94 19.065 25.625 17.623 22.794L19.823 20.587C20.098 20.313 20.179 19.918 20.068 19.572C19.7 18.447 19.499 17.247 19.499 16C19.499 15.447 19.052 15 18.5 15H15Z" fill="white"/>
        </svg>

        <div class="modal-details">
            <p class="modal-details__name">{{ selectedUser.name.first + ' ' + selectedUser.name.last}}</p>
            <p class="user-overview__location">Calling...</p>
        </div>
      </div>
    </div>
</template>

<script>
/*jshint esversion: 6 */
export default {
    name: 'Modal',

    props: {
        selectedUser: Object,
        show: Boolean
    },

    data: function() {
        return {
            open: false,
        }
    },

    methods: {
        closeModal: function() {
          this.$emit('close');
        }
    }
}
</script>

<style lang="scss">
.modal {
  background-position: center;
  background-size: cover;
  height: 100vh;
  left: 0;
  position: fixed;
  top: 0;
  width: 100vw;
  z-index: 100;
}

.modal-inner {
  align-items: flex-end;
  box-sizing: border-box;
  display: flex;
  justify-content: center;
  margin: 75vh auto 0;
  padding: 0 30px;
  padding: 10px;

  svg {
    cursor: pointer;
    transition: transform .5s ease;

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
}

.modal-details {
  color: white;
  margin-left: 30px;
  text-align: left;
  
  &__name {
      font-size: 1.31rem;
      font-weight: bold;
  }
  p {
    margin: 0;
  }
}
</style>

