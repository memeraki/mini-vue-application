<template>
  <div class="header">
    <div class="searchBar">
      <input type="text" placeholder="search" >
    </div>
    <Favourities />
  </div>
</template>

<script>
import Favourities from './Favourities.vue';

export default {
  components: {
    Favourities
  }
}
</script>

<style>
  .header {
    grid-area: header;
    display: flex;
    flex-flow: row nowrap;
    justify-content: space-evenly;
    width: 100%;
    padding: 25px;
    background-color: lightyellow;
  }
</style>
