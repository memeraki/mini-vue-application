<template>
  <div class="sidebar">
    <div>
      <h2>Category</h2>
      <ul>
        <li v-for="(categoryName, index) in categories" :key="index">
          {{ categoryName }}
        </li>
      </ul>
    </div>
    <div>
      <h2>Area</h2>
      <ul>
        <li v-for="(areaName, index) in areas" :key="index">
          {{ areaName }}
        </li>
      </ul>
    </div>
    <div>
      <h2>Tags</h2>
      <ul>
        <li v-for="(tagName, index) in tags" :key="index">
          {{ tagName }}
        </li>
      </ul>
    </div>
  </div>
</template>

<script>
export default {
  props: ['categories', 'areas', 'tags'],
  methods: {

  }
}
</script>

<style>
  .sidebar {
    grid-area: sidebar;
    padding: 10px;
    background-color: darkgrey;
    display: flex;
    flex-flow: column nowrap;
    justify-content: space-evenly;
  }
  ul {
    list-style: none;
  }

  li {
    border: 1px solid black;
    border-radius: 5px;
    margin: 2px;
  }
</style>