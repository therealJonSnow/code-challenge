<template>
  <div class="filters">
    
    <p class="filter filter--active" @click="changeFilter" data-filter-parent="name" data-filter-child="first">Name</p>
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
        e.classList.remove('filter--active');
      });
      event.target.classList.add('filter--active');
      this.$emit('clicked', [event.target.dataset.filterParent, event.target.dataset.filterChild] );
    }
  }
}
</script>

<style lang="scss">
.filters{
  display: flex;
  justify-content: center;
  padding-bottom: 1rem;

  .filter {
    color: #EEEEEE;
    cursor: pointer;
    font-family: Roboto;
    font-size: 14px;
    font-style: normal;
    font-weight: 500;
    letter-spacing: 0.0357143em;
    line-height: 16px;
    margin: 0 10px;
    padding: .5rem;
    text-align: center;
    transition: all .5s ease;

    &--active {
      background: rgba(153, 153, 153, 0.5);
      border-radius: 5px;
      transition: all .5s ease;
    }
  }
}
</style>