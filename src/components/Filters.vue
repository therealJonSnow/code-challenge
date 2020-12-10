<template>
  <div class="filter-dropdown">
    
    <p class="filter active" @click="changeFilter" data-filter-parent="name" data-filter-child="first">Name</p>
    <p class="filter" @click="changeFilter" data-filter-parent="name" data-filter-child="last">Surame</p>
    <p class="filter" @click="changeFilter" data-filter-parent="location" data-filter-child="country">Country</p>

  </div>
</template>

<script>
export default {
  methods: {
    changeFilter (event) {
      var filters = document.getElementsByClassName('filter');
      filters.forEach(e => {
        e.classList.remove('active');
      });
      event.target.classList.add('active');
      
      this.$emit('clicked', [event.target.dataset.filterParent, event.target.dataset.filterChild] );
    }
  }
}
</script>

<style lang="scss">
.filter-dropdown{
  display: flex;
  justify-content: center;
  padding-bottom: 1rem;
  

  .filter {
    cursor: pointer;
    padding: .5rem;
    margin: 0 10px;
    font-family: Roboto;
    font-style: normal;
    font-weight: 500;
    font-size: 14px;
    line-height: 16px;
    text-align: center;
    letter-spacing: 0.0357143em;
    color: #EEEEEE;


    &.active {
      background: rgba(153, 153, 153, 0.5);
      border-radius: 5px;
    }
  }
}
</style>